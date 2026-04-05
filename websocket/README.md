# 📘 Backend & Real-Time Interview Q&A

> A developer reference for common backend, WebSocket, and system design interview questions — with practical answers.

---

## 📑 Table of Contents

1. [WebSocket: Connection Loss & Message Recovery](#1-websocket-connection-loss--message-recovery)
2. [WebSocket vs HTTP Polling — When to Use Which?](#2-websocket-vs-http-polling--when-to-use-which)
3. [How Does a WebSocket Handshake Work?](#3-how-does-a-websocket-handshake-work)
4. [How Do You Handle WebSocket Authentication?](#4-how-do-you-handle-websocket-authentication)
5. [How Do You Scale WebSocket Servers Horizontally?](#5-how-do-you-scale-websocket-servers-horizontally)
6. [How Do You Handle WebSocket Reconnection with Exponential Backoff?](#6-how-do-you-handle-websocket-reconnection-with-exponential-backoff)
7. [STUN and TURN Servers (WebRTC)](#7-stun-and-turn-servers-webrtc)
8. [Duplicate Webhooks from a Payment Gateway](#8-duplicate-webhooks-from-a-payment-gateway)
9. [Scaling an API from 100 to 10,000 req/sec](#9-scaling-an-api-from-100-to-10000-reqsec)
10. [Large Excel Export/Import Without Timeouts](#10-large-excel-exportimport-without-timeouts)

---

## 1. WebSocket: Connection Loss & Message Recovery

**Q: How would you handle a 5-minute connection loss and restore missed 1:1 messages?**

You don't "recover" messages from the WebSocket itself — you design the system so messages are **durably stored and replayable**. The WebSocket is just a transport layer.

---

### 💡 Core Idea

Every message must be:
- ✅ Persisted on the server (DB)
- ✅ Uniquely ordered (ID or timestamp)
- ✅ Fetchable via an API

---

### 🔌 On Disconnect

- Detect drop via **ping/pong** or heartbeat timeout
- Mark client as **offline**
- Keep storing messages to DB as usual — never depend on socket delivery

---

### 🗄️ While User is Offline

Messages stored with:

```
conversation_id
sender_id
message_id        ← auto-increment or UUID + timestamp
created_at
status            ← sent | delivered | read  (optional)
```

---

### 🔄 On Reconnect

**Client sends:**
```json
{ "last_received_message_id": 456 }
```

**Server queries:**
```sql
SELECT * FROM messages
WHERE conversation_id = X
  AND message_id > 456
ORDER BY message_id ASC
```

**Server responds:**
```json
{ "messages": [457, 458, 459] }
```

---

### 🔁 Client Recovery Flow

1. Reconnect WebSocket
2. Call REST endpoint or emit socket event: `"give me messages after ID 456"`
3. Merge into UI — deduplicate by `message_id`

---

### 🛡️ Extra Reliability Patterns

| Pattern | Purpose |
|---|---|
| **ACK system** | Client acknowledges receipt; server retries if needed |
| **Idempotency** | Prevent duplicate messages on reconnect |
| **Message queue** | Kafka / Redis Streams for scaling |
| **Backfill window** | Always sync from last known point, not just 5 min |

---

### 🧠 Key Principle

```
WebSocket  →  real-time delivery
DB / API   →  source of truth + recovery
```

> If you rely only on WebSockets, you **will** lose messages. This design guarantees you won't.

---

## 2. WebSocket vs HTTP Polling — When to Use Which?

**Q: When should you use WebSockets vs HTTP polling (short or long)?**

---

### ⚡ WebSocket

A **persistent, full-duplex** connection between client and server. Both sides can send messages at any time.

**Use when:**
- Real-time chat / messaging
- Live notifications
- Multiplayer games / crash games
- Live dashboards (stock prices, sports scores)
- Collaborative editing

```
Client ←──────────────────── Server
       ──── persistent TCP ────
```

**Pros:**
- Very low latency
- No repeated HTTP overhead
- Server can push instantly

**Cons:**
- More complex to scale (sticky sessions / pub-sub needed)
- Harder to debug than REST
- Not ideal for infrequent updates

---

### 🔁 Short Polling

Client sends an HTTP request every N seconds regardless of new data.

```javascript
setInterval(() => {
  fetch('/api/messages').then(res => updateUI(res));
}, 3000);
```

**Use when:**
- Very simple use case
- Infrequent updates (every 30s+)
- You don't want WebSocket complexity

**Cons:** Wastes bandwidth, high server load, not truly real-time.

---

### ⏳ Long Polling

Client sends a request → server holds it open until there's data (or timeout), then client immediately re-requests.

**Use when:**
- Fallback for environments that block WebSockets
- Low-to-medium update frequency

**Cons:** Higher latency than WebSockets, complex server-side management.

---

### 📊 Comparison Table

| Feature | WebSocket | Long Polling | Short Polling |
|---|---|---|---|
| Latency | ⚡ Very low | Medium | High |
| Server load | Low (persistent) | Medium | High |
| Complexity | High | Medium | Low |
| Best for | Real-time apps | Fallback | Rare updates |
| Server push | ✅ Yes | ✅ Yes | ❌ No |

---

### 🎯 Interview Answer

> "I use WebSockets for anything truly real-time — chat, games, live feeds. For simpler use cases or environments where WebSockets are blocked, long polling is a clean fallback. Short polling is a last resort."

---

## 3. How Does a WebSocket Handshake Work?

**Q: Explain the WebSocket handshake process.**

WebSocket starts as an HTTP request and **upgrades** to a persistent connection.

---

### 🤝 Step-by-Step

**Step 1 — Client sends HTTP Upgrade request:**
```http
GET /chat HTTP/1.1
Host: example.com
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
Sec-WebSocket-Version: 13
```

**Step 2 — Server responds with 101 Switching Protocols:**
```http
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: s3pPLMBiTxaQ9kYGzzhZRbK+xOo=
```

**Step 3 — Connection is now open:**
```
Client ←─── full-duplex TCP channel ───→ Server
```

Both sides can now send frames (text, binary, ping/pong, close) at any time.

---

### 🔐 Key Header: `Sec-WebSocket-Accept`

The server takes the client's `Sec-WebSocket-Key`, appends a magic GUID, SHA-1 hashes it, and base64 encodes it. This proves the server understands the WebSocket protocol.

---

### 🎯 Interview Answer

> "WebSocket starts as a standard HTTP request with an `Upgrade: websocket` header. The server responds with `101 Switching Protocols`, and from that point, the TCP connection stays open for bidirectional communication — no more HTTP overhead per message."

---

## 4. How Do You Handle WebSocket Authentication?

**Q: How do you authenticate a WebSocket connection securely?**

You can't send custom headers during a browser WebSocket upgrade (unlike REST). So authentication needs a different approach.

---

### ✅ Recommended Approaches

**Option 1 — Token in Query String (common, simple):**
```javascript
const socket = io('wss://example.com', {
  query: { token: 'eyJhbGciOiJIUzI1NiJ9...' }
});
```
Server validates token on connection:
```javascript
io.use((socket, next) => {
  const token = socket.handshake.query.token;
  try {
    const user = jwt.verify(token, process.env.JWT_SECRET);
    socket.user = user;
    next();
  } catch (e) {
    next(new Error('Unauthorized'));
  }
});
```

> ⚠️ Query string tokens appear in server logs. Use HTTPS always and short-lived tokens.

---

**Option 2 — Cookie-based (most secure for browsers):**

If your REST API already uses HTTP-only cookies for auth, the browser automatically sends them on the WebSocket upgrade request. Validate the cookie/session server-side.

---

**Option 3 — Auth event after connect:**

Client connects first, then immediately sends auth event:
```javascript
socket.emit('authenticate', { token: 'abc123' });

socket.on('authenticated', () => {
  // now safe to use socket
});
```
Server disconnects if no auth event arrives within N seconds.

---

### 🎯 Interview Answer

> "The cleanest approach is validating a JWT in the Socket.io middleware during the handshake — either from a query param or cookie. I use a middleware that runs before any event is processed, so unauthenticated connections are rejected before they can do anything."

---

## 5. How Do You Scale WebSocket Servers Horizontally?

**Q: WebSockets are stateful — how do you run multiple instances?**

The problem: if you have 3 server instances and User A is connected to Instance 1, but User B is on Instance 3 — Instance 1 can't directly emit to User B.

---

### 🔴 The Problem

```
User A ──→ Instance 1
User B ──→ Instance 3   ← Instance 1 can't reach this
```

---

### ✅ Solution: Pub/Sub Adapter (Redis)

All instances subscribe to a shared Redis channel. When any instance emits to a room/user, Redis broadcasts it to all instances.

```
Instance 1 ──publish──→ Redis ──→ Instance 2
                               ──→ Instance 3
```

**Socket.io with Redis adapter:**
```javascript
const { createAdapter } = require('@socket.io/redis-adapter');
const { createClient } = require('redis');

const pubClient = createClient({ url: 'redis://localhost:6379' });
const subClient = pubClient.duplicate();

io.adapter(createAdapter(pubClient, subClient));
```

Now all instances share event broadcasting automatically.

---

### ⚖️ Load Balancer Configuration

WebSocket requires **sticky sessions** (same client always routes to same instance) OR the Redis adapter must be used so any instance can handle any event.

With Redis adapter → sticky sessions not required.

With NGINX:
```nginx
upstream socketio {
  ip_hash;  # sticky sessions
  server instance1:3000;
  server instance2:3000;
}
```

---

### 🎯 Interview Answer

> "WebSockets are stateful, so horizontal scaling requires a pub/sub layer. I use Socket.io with the Redis adapter — all instances publish events to Redis and subscribe to receive them, so a message sent from Instance 1 reaches a user connected to Instance 3 transparently."

---

## 6. How Do You Handle WebSocket Reconnection with Exponential Backoff?

**Q: How should a client handle reconnecting after a WebSocket drops?**

Never reconnect immediately in a tight loop — you'll hammer the server during an outage.

---

### ✅ Exponential Backoff Strategy

Wait longer after each failed attempt, with a random jitter to avoid thundering herd.

```javascript
let attempt = 0;
const MAX_DELAY = 30000; // 30 seconds cap

function reconnect() {
  const delay = Math.min(1000 * Math.pow(2, attempt), MAX_DELAY);
  const jitter = Math.random() * 1000; // random 0–1s jitter

  console.log(`Reconnecting in ${Math.round(delay + jitter)}ms (attempt ${attempt + 1})`);

  setTimeout(() => {
    attempt++;
    connectWebSocket();
  }, delay + jitter);
}

function connectWebSocket() {
  const ws = new WebSocket('wss://example.com');

  ws.onopen = () => {
    attempt = 0; // reset on success
    syncMissedMessages(); // fetch missed messages after reconnect
  };

  ws.onclose = () => reconnect();
  ws.onerror = () => reconnect();
}
```

---

### 📈 Backoff Timeline

| Attempt | Delay (approx) |
|---|---|
| 1 | 1s |
| 2 | 2s |
| 3 | 4s |
| 4 | 8s |
| 5 | 16s |
| 6+ | 30s (capped) |

---

### 🎯 Interview Answer

> "On disconnect, I implement exponential backoff with jitter — starting at 1 second, doubling each attempt, capped at 30 seconds. After reconnecting, the client immediately syncs missed messages from the last known message ID. Socket.io handles this automatically, but it's important to understand what's happening under the hood."

---

## 7. STUN and TURN Servers (WebRTC)

**Q: Do you know what STUN and TURN servers are?**

Yes — they're key pieces of WebRTC networking for peer-to-peer connections.

---

### 🔍 STUN (Session Traversal Utilities for NAT)

Helps a device discover its **public IP and port** when it's behind a NAT.

- Think of it as: *"What do I look like to the internet?"*
- Used to attempt direct peer-to-peer connections
- Lightweight and fast
- Does **not** relay data — only assists with discovery

---

### 🔁 TURN (Traversal Using Relays around NAT)

Acts as a **relay server** when direct P2P connection fails.

- If strict NAT/firewalls block P2P, all traffic routes through TURN
- More reliable but adds latency and cost (all media flows through it)

---

### 🔄 How They Work Together in WebRTC

```
1. Client uses STUN → gathers public IP candidates
2. Peers attempt direct connection (ICE process)
3. If P2P fails → fallback to TURN relay
```

```
Client A ──→ STUN: "what's my public IP?"
Client A ←── STUN: "203.0.113.5:54321"

Client A ←──── P2P attempt ────→ Client B
                  ↓ fails
Client A ←── TURN relay ──→ Client B
```

---

### 📊 Summary

| | STUN | TURN |
|---|---|---|
| Purpose | Discover public IP | Relay traffic |
| Relays media | ❌ No | ✅ Yes |
| Cost | Very low | Higher (bandwidth) |
| Latency | Low | Higher |
| When used | Always (first attempt) | Fallback |

> **Rule of thumb:** ~85% of connections succeed via STUN (P2P). The remaining ~15% need TURN. Always configure both for real-world reliability.

---

### 🎯 Interview Answer

> "STUN helps clients discover their public-facing IP/port for direct P2P connections. TURN is a relay fallback for when P2P is blocked by strict NAT or firewalls. In WebRTC, you configure both — STUN is tried first, TURN is the safety net."

---

## 8. Duplicate Webhooks from a Payment Gateway

**Q: How do you handle duplicate webhooks from a payment gateway?**

**Short answer:** Assume duplicates **will** happen. Make your webhook handler **idempotent**.

---

### ✅ Step-by-Step Approach

**1. Use the gateway's unique event ID**

Most gateways (Stripe, Razorpay, etc.) send a unique `event_id` or `payment_id`:
- Store it in your DB with a `UNIQUE` constraint
- Before processing: check if it already exists

**2. Enforce idempotency at DB level**

Don't rely only on code-level checks:
```sql
CREATE UNIQUE INDEX idx_event_id ON webhook_events(event_id);
```
On duplicate insert → DB rejects → treat as already processed.

**3. Track processing state**

```sql
webhook_events table:
  event_id     VARCHAR  UNIQUE
  status       ENUM('pending', 'processed', 'failed')
  processed_at TIMESTAMP
```

**4. Make operations idempotent**

Even if the same webhook runs twice, nothing breaks:
- Use `UPDATE` instead of `INSERT` where possible
- Check if order is already marked `paid` before updating

**5. Use locking for race conditions**

If high concurrency (multiple server instances):
```sql
SELECT * FROM webhook_events WHERE event_id = ? FOR UPDATE;
```
Or use distributed locks via Redis.

**6. Return `2xx` quickly**

Gateways retry if they don't receive success. Acknowledge fast, process async:
```javascript
app.post('/webhook', async (req, res) => {
  res.status(200).send('OK'); // acknowledge immediately
  await queue.add('process-webhook', req.body); // async processing
});
```

---

### 🔁 Simple Flow

```
Receive webhook
    ↓
Validate signature
    ↓
Check event_id in DB
    ↓
exists? → return 200 (skip)
    ↓
not exists? → process + store → return 200
```

---

### 🎯 Interview Answer

> "I treat webhook handlers as idempotent by design. I store the gateway's event_id with a unique DB constraint, check it before processing, and return 200 immediately even for duplicates. For race conditions with multiple instances, I add a DB row lock or Redis lock before processing."

---

## 9. Scaling an API from 100 to 10,000 req/sec

**Q: How would you scale an API from 100 requests/sec to 10,000 requests/sec?**

At that scale jump, you don't "tune" your way there — you **redesign for horizontal scale**.

---

### 🪜 Step-by-Step Strategy

**1. Remove Single Points of Failure**
- Load balancer (NGINX, HAProxy, cloud LB) in front
- Multiple stateless API instances (containers / VMs)
- App servers must be disposable — no local state

**2. Horizontal Scaling**
- Auto-scaling groups / Kubernetes HPA behind the LB
- Stateless requests: any instance can handle any request

**3. Aggressive Caching**
- **Redis/Memcached:** DB query results, session/token validation
- **CDN (Cloudflare):** Edge caching for read-heavy endpoints
- Cache at multiple layers: edge → app → DB

**4. Database Bottleneck Fixes**
- **Read replicas** for scaling reads
- **Partitioning / sharding** for heavy datasets
- **Query optimization + indexes** (often gives 10x alone)
- **Connection pooling** (PgBouncer, RDS Proxy)

**5. Async & Queue-Based Processing**
- Offload slow work: emails, notifications, file processing
- Queues: Kafka, RabbitMQ, SQS, Laravel queues
- Keep API response time under 100ms

**6. Rate Limiting & Backpressure**
- Token bucket / leaky bucket via Redis or API gateway
- Protect from traffic spikes

**7. Optimize App Layer**
- Non-blocking I/O: Node.js, Swoole/RoadRunner for PHP
- Payload compression (gzip)
- Pagination — never return unbounded datasets
- Fix N+1 queries

**8. API Gateway Layer**
- Centralize: auth, rate limiting, routing, logging
- Tools: Kong, AWS API Gateway, NGINX

**9. Observability**
- Metrics: RPS, latency p95/p99, error rate
- Tools: Prometheus + Grafana, ELK stack
- Load test continuously: k6, Locust

---

### 📈 Gradual Scale Milestones

| Range | Focus |
|---|---|
| 100 → 1,000 | Query optimization + Redis caching |
| 1,000 → 5,000 | Read replicas + horizontal app scaling |
| 5,000 → 10,000 | DB sharding + queues + CDN + API gateway |

---

### 🗺️ For a Node.js + Laravel Stack

- Put **Laravel** behind NGINX + Redis cache
- Move heavy jobs to **Laravel queues + workers**
- Add **read replicas** for MySQL
- Keep **Node.js/Socket.io** as a separate real-time layer
- Add **CDN + API gateway** early (easier to add than retrofit)

---

### 🎯 Interview Answer

> "I'd approach it in phases: first optimize queries and add Redis caching, then scale horizontally behind a load balancer, then add read replicas and async queues for slow operations. At 10k RPS, you also need a CDN, an API gateway for rate limiting, and full observability to find bottlenecks before they become outages."

---

## 10. Large Excel Export/Import Without Timeouts

**Q: How should large Excel exports/imports be handled to avoid timeouts?**

Timeouts happen because large Excel operations are **CPU, memory, and I/O heavy**. The fix is to stop doing them synchronously in a single HTTP request.

---

### 📤 For Exports

**Queue the job:**
Generate the file in a background worker — not in the HTTP request:
```javascript
// ✅ Correct
app.get('/export', async (req, res) => {
  const jobId = await queue.add('generate-excel', { filters: req.query });
  res.json({ jobId, status: 'processing' });
});
```

**Stream the output:**
Write rows incrementally using chunked queries:
```php
// Laravel Excel with chunked query
public function query() {
  return Model::query()->chunk(1000);
}
```

**Store, don't return:**
- Save the file to S3 / local storage
- Send user a download link when ready (email, notification, polling)

**Chunk DB reads:**
Process in batches of 1k–5k rows to avoid memory spikes.

---

### 📥 For Imports

```
Upload file → Store (S3/disk) → Queue job → Process in chunks → Report results
```

- **Chunk reading:** Never load the whole file into memory
- **Batch inserts:** Use bulk `INSERT` instead of per-row queries
- **Batch validation:** Validate per chunk and log failures (don't stop everything on one error)
- **Retry failed chunks** independently

---

### ⚙️ General Optimizations

| Tip | Why |
|---|---|
| Prefer CSV over XLSX | Much faster and lighter to parse |
| Progress tracking | Status table + polling endpoint or WebSocket push |
| Increase memory/timeout | Last resort, not the real fix |
| Idempotent import | Safe to re-run if it fails halfway |

---

### 🎯 Interview Answer

> "The golden rule is: never do large file operations synchronously. I queue the job, chunk the DB reads, stream writes incrementally, store the result on S3, and notify the user when it's ready. This way, no matter how large the file is, the HTTP response is always fast."

---

*Last updated: 2026 · Maintained for backend & real-time interview preparation*