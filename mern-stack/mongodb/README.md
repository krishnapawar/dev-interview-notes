# 🍃 MongoDB & System Design — Interview Questions & Answers

## 📋 Table of Contents
1. [When would you use $lookup?](#1-when-would-you-use-lookup)
2. [Difference between findOne() and aggregate().match().limit(1)?](#2-difference-between-findone-and-aggregatematchlimit1)
3. [How would you design a schema for an e-commerce cart system?](#3-how-would-you-design-a-schema-for-an-e-commerce-cart-system)
4. [How Would You Design a Scalable Chat App with Typing Indicators?](#4-how-would-you-design-a-scalable-chat-app-with-typing-indicators)
5. [Difference Between SQL and NoSQL Databases?](#5-difference-between-sql-and-nosql-databases)
6. [What Was the Biggest Challenge in Your Project and How Did You Solve It?](#6-what-was-the-biggest-challenge-in-your-project-and-how-did-you-solve-it)

---

## 1. When would you use $lookup?

> **Interview-Ready Answer:**  
> Use `$lookup` in MongoDB aggregation pipelines to perform a left outer join with another collection in the same database, allowing you to combine related documents (such as linking orders to customer profiles) into an output array field.

`$lookup` is used to join collections (similar to SQL joins).

**Example:** Getting orders with customer details.

```javascript
db.orders.aggregate([
  { 
    $lookup: {
      from: "customers",
      localField: "customerId",
      foreignField: "_id",
      as: "customer"
    }
  }
]);
```

---


---

## 2. Difference between findOne() and aggregate().match().limit(1)?

> **Interview-Ready Answer:**  
> `findOne()` executes a direct, lightweight index lookup and stops immediately upon finding the first document, whereas `aggregate().match().limit(1)` initiates the multi-stage aggregation pipeline engine, incurring higher overhead but offering rich data transformation capabilities.

- **findOne()** → quick, optimized, returns first matching doc.
- **aggregate().match().limit(1)** → heavier, goes through pipeline, more flexible for complex queries (e.g., transformations).

---


---

## 3. How would you design a schema for an e-commerce cart system?

> **Interview-Ready Answer:**  
> Design an e-commerce cart schema by embedding an array of items (`productId`, `quantity`, `priceAtAddition`) directly inside an active `cart` document per user, referencing product IDs for inventory checks while maintaining fast single-read cart retrieval.

**Users collection:**

```json
{ name, email, password }
```

**Products collection:**

```json
{ name, price, stock }
```

**Cart collection:**

```json
{
  userId,
  items: [
    { productId, quantity, price }
  ],
  totalAmount,
  status: "active" | "ordered"
}
```

Cart embedded items for fast access.

---


---

## 4. How Would You Design a Scalable Chat App with Typing Indicators?

> **Interview-Ready Answer:**  
> Build a scalable real-time chat architecture by decoupling WebSocket servers via Redis Pub/Sub, persisting chat history in MongoDB with indexed compound keys (`conversationId` + `createdAt`), and emitting ephemeral in-memory typing indicator events directly through Socket.IO without touching the database.

**Frontend:** React + Socket.IO  

**Backend:** Node.js + Express + Socket.IO  

**Database:** MongoDB for storing messages, Redis for caching online users

**Flow:**

- User connects via WebSocket → stored in Redis
- Messages sent → saved in MongoDB, emitted to recipient
- Typing indicators → socket events (`typing`, `stopTyping`)
- For scaling → use Redis pub/sub with multiple Node instances

---


---

## 5. Difference Between SQL and NoSQL Databases?

> **Interview-Ready Answer:**  
> SQL databases (MySQL, PostgreSQL) provide rigid schemas, relational foreign keys, and strong ACID guarantees ideal for financial systems; NoSQL databases (MongoDB) provide flexible JSON document schemas, native sharding, and high horizontal scalability ideal for rapid iteration and high-write workloads.

- **SQL** → structured, relational, ACID, best for banking, inventory.
- **NoSQL (MongoDB)** → flexible schema, horizontal scaling, best for fast-changing data, chat apps, analytics.

---


---

## 6. What Was the Biggest Challenge in Your Project and How Did You Solve It?

> **Interview-Ready Answer:**  
> The biggest challenge was API latency spiking to over 1.2s under heavy load. I diagnosed the bottleneck using MongoDB's `explain('executionStats')`, created targeted compound indexes to eliminate COLLSCANs, and introduced Redis caching for read-heavy queries, driving response times down to under 150ms.

**Example (you can adapt):**

In my last project, we faced performance issues with MongoDB queries as data grew. I analyzed slow queries with `explain()`, created proper indexes, and implemented Redis caching. This reduced API response time from ~1.2s to ~150ms.

