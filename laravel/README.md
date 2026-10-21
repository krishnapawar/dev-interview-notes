# 🔴 Laravel — Interview Questions & Answers

## 📋 Table of Contents
1. [What Is Service Container?](#1-what-is-service-container)
2. [What Is Middleware?](#2-what-is-middleware)
3. [Difference Between Eager and Lazy Loading](#3-difference-between-eager-and-lazy-loading)
4. [What Is CSRF?](#4-what-is-csrf)
5. [What Is Queue in Laravel?](#5-what-is-queue-in-laravel)

---

## 1. What Is Service Container?

### 💡 Answer

Laravel’s Dependency Injection container that automatically resolves and manages class dependencies.

---

## 2. What Is Middleware?

### 💡 Answer

Middleware filters HTTP requests.

**Examples:**

- Authentication  
- Role checking  
- Logging  

---

## 3. Difference Between Eager and Lazy Loading

### 💡 Answer

- **Lazy Loading** → Loads relation when accessed  
- **Eager Loading** → Loads relation with main query using `with()`  

Eager loading prevents the **N+1 problem**.

---

## 4. What Is CSRF?

### 💡 Answer

CSRF (Cross-Site Request Forgery) is a security attack.

Laravel protects against CSRF using a **CSRF token**.

---

## 5. What Is Queue in Laravel?

### 💡 Answer

Queues are used for background jobs like:

- Email sending  
- Notifications  
- Heavy processing  

Queues improve application performance by making tasks asynchronous.