----

## 1. HTTP Method vs Route (the core idea)

Think of an HTTP request as answering **two questions**:

### 1️⃣ What do you want to do? → **HTTP Method**

This is your **intent / action**.

|Method|Intent|
|---|---|
|GET|Fetch data|
|POST|Create data|
|PUT / PATCH|Update data|
|DELETE|Remove data|

---

### 2️⃣ Where do you want to do it? → **Route (URL path)**

This identifies **which resource** the action applies to.

Example:

```
GET /api/books
```

- `GET` → fetch
    
- `/api/books` → books resource
    

---

### 🔑 Routing = Method + Path → Handler

On the server:

```
(GET, /api/books)  → handler A
(POST, /api/books) → handler B
```

Even though the **route path is the same**, the **method makes them different**.

➡️ **Method + route together form a unique key**  
➡️ They will **never clash**

---

## 2. Static Routes

### Definition

Routes with **no variables**.

Example:

```
GET  /api/books
POST /api/books
```

- Path never changes
    
- Always returns the same _type_ of data
    
- Easy to reason about


![[Pasted image 20260206173301.png]]
![[Pasted image 20260206173330.png]]



---

## 3. Dynamic Routes (Path Parameters)

### Why needed?

To act on **a specific resource**, not the whole collection.

Example:

```
GET /api/users/123
```

### How server defines it

```js
GET /api/users/:id
```

- `:id` = **path parameter**
    
- Can match **any string**
    
- Values are extracted and used in logic
    

```js
req.params.id === "123"
```

### Semantic meaning (very important for REST)

```
GET /api/users/123
→ Fetch user whose ID is 123
```

Human-readable, intention-revealing URLs are the whole point of REST.


![[Pasted image 20260206173532.png]]

- **GET /api/users/123** : Here '123' is the parameter (user id in this example). This is an example of **Path Parameter**.

---

## 4. Query Parameters

### Syntax

```
GET /api/search?query=someValue
```

- Everything **after `?`**
    
- Key–value pairs
    
- Optional metadata
    

### Why query params exist

- **GET requests don’t have a body**
    
- Path parameters are for **identity**
    
- Query params are for **filtering, sorting, pagination**
    

### Common uses

```
/api/books?page=2
/api/books?limit=20
/api/books?sort=price&order=asc
```

### Rule of thumb

- **Path param** → _Which resource?_
    
- **Query param** → _How do you want it?_


![[Pasted image 20260206173858.png]]

**GET /api/search?query=some+value** : This is an query parameter. GET /api/search is the route used to map to a handler, the key value pair of the query is used to further specify details.

---

## 5. Nested Routes

### Why nesting exists

Resources often **belong to other resources**.

Example:



![[Pasted image 20260206174943.png]]

```
GET /api/users/123/posts/456
```

### Semantic meaning

> Fetch post **456** belonging to user **123**

### Levels of nesting

Each level changes the meaning:

|Route|Meaning|
|---|---|
|`/api/users`|All users|
|`/api/users/123`|One user|
|`/api/users/123/posts`|All posts of user|
|`/api/users/123/posts/456`|One specific post|

This is **not a special routing type**, just a **REST best practice**.

---

## 6. Route Versioning & Deprecation

### Problem it solves

APIs evolve → response formats change → clients break

### Solution

Version the API:

```
/api/v1/products
/api/v2/products
```

Example difference:

```json
// v1
{ "id": 1, "name": "Book", "price": 100 }

// v2
{ "id": 1, "title": "Book", "price": 100 }
```

### Benefits

- No breaking existing clients
    
- Clear migration path
    
- Old versions can be deprecated safely
    

---

## 7. Catch-All Route (404 handling)

### Purpose

Handle **unknown routes gracefully**

Example:

```
GET /api/v3/products
```

Server doesn’t support it → instead of crashing:

```
404 Route not found
```

### Typical implementation

```js
app.use("*", handler)
```

- Matches anything not handled earlier
    
- Always placed **at the end**
    

---

## 8. One-line Summary (gold)

> **Routing is the process of mapping an HTTP method + URL path (with static, dynamic, and nested parameters) to a specific server-side handler.**

---
