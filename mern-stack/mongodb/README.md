# 🍃 MongoDB & System Design — Interview Questions & Answers

## 📋 Table of Contents
1. [When would you use $lookup?](#q10-when-would-you-use-lookup)
2. [Difference between findOne() and aggregate().match().limit(1)?](#q11-difference-between-findone-and-aggregatematchlimit1)
3. [How would you design a schema for an e-commerce cart system?](#q12-how-would-you-design-a-schema-for-an-e-commerce-cart-system)
4. [Design a scalable chat app with typing indicators](#q13-design-a-scalable-chat-app-with-typing-indicators)
5. [SQL vs NoSQL?](#q14-sql-vs-nosql)
6. [Biggest challenge in your project?](#q15-biggest-challenge-in-your-project)

---

### Q10: When would you use $lookup?

**Answer:**

`$lookup` is used to join collections (similar to SQL joins).

Example: Getting orders with customer details.

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

### Q11: Difference between findOne() and aggregate().match().limit(1)?

**Answer:**

- **findOne()** → quick, optimized, returns first matching doc.
- **aggregate().match().limit(1)** → heavier, goes through pipeline, more flexible for complex queries (e.g., transformations).

---

### Q12: How would you design a schema for an e-commerce cart system?

**Answer:**

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

### Q13: Design a scalable chat app with typing indicators.

**Answer:**

**Frontend:** React + Socket.IO  

**Backend:** Node.js + Express + Socket.IO  

**Database:** MongoDB for storing messages, Redis for caching online users

**Flow:**

- User connects via WebSocket → stored in Redis
- Messages sent → saved in MongoDB, emitted to recipient
- Typing indicators → socket events (`typing`, `stopTyping`)
- For scaling → use Redis pub/sub with multiple Node instances

---

### Q14: SQL vs NoSQL?

**Answer:**

- **SQL** → structured, relational, ACID, best for banking, inventory.
- **NoSQL (MongoDB)** → flexible schema, horizontal scaling, best for fast-changing data, chat apps, analytics.

---

### Q15: Biggest challenge in your project?

**Answer (example you can adapt):**

In my last project, we faced performance issues with MongoDB queries as data grew. I analyzed slow queries with `explain()`, created proper indexes, and implemented Redis caching. This reduced API response time from ~1.2s to ~150ms.
