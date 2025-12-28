## 1. What is Express.js?

**Express.js** is a **minimal and flexible web framework** built on top of **Node.js**.

* It simplifies building **web servers** and **REST APIs**
* Provides easy routing, middleware, and request/response handling
* Runs on Node.js but reduces boilerplate code

**Node.js = engine**
**Express.js = framework on that engine**

---

## 2. Why Express over plain Node.js?

### Problem with pure Node.js

Creating a server in Node.js requires:

* Manual routing
* Manual request parsing
* Repetitive code
* Hard-to-manage logic as app grows

### Example (Node.js only)

```js
const http = require("http");

const server = http.createServer((req, res) => {
  if (req.url === "/") {
    res.end("Home");
  } else if (req.url === "/about") {
    res.end("About");
  } else {
    res.end("404");
  }
});

server.listen(3000);
```

### Same thing in Express

```js
const express = require("express");
const app = express();

app.get("/", (req, res) => res.send("Home"));
app.get("/about", (req, res) => res.send("About"));

app.listen(3000);
```

**Less code, more readability, more power**

---

## 3. Why a framework is needed?

Frameworks solve:

* Code structure
* Reusability
* Scalability
* Security
* Maintainability

Without a framework:

* Logic becomes messy
* Difficult to scale
* No standard flow

Express provides:

* Routing system
* Middleware pipeline
* Error handling
* Request/Response abstraction

---

## 4. Express Architecture (High Level)

```
Client
  ↓
Request
  ↓
Middleware
  ↓
Route Handler
  ↓
Response
  ↓
Client
```

### Core components

1. **Request (req)**
2. **Response (res)**
3. **Middleware**
4. **Routes**
5. **Server (app.listen)**

---

## 5. Request–Response Lifecycle in Express

1. Client sends HTTP request
2. Express receives request
3. Middleware runs (auth, logging, parsing)
4. Route handler executes
5. Response is sent back
6. Request ends

### Example flow

```js
app.use((req, res, next) => {
  console.log("Middleware");
  next();
});

app.get("/", (req, res) => {
  res.send("Hello");
});
```

Order matters.

---

## 6. Create First Express Server

### Step 1: Install Express

```bash
npm init -y
npm install express
```

### Step 2: Create server (index.js)

```js
const express = require("express");

const app = express();

app.listen(3000, () => {
  console.log("Server running on port 3000");
});
```

---

## 7. Basic Routes in Express

### GET route

```js
app.get("/", (req, res) => {
  res.send("Home Page");
});
```

### POST route

```js
app.post("/login", (req, res) => {
  res.send("Login Success");
});
```

### Route with params

```js
app.get("/user/:id", (req, res) => {
  res.send(req.params.id);
});
```

### Query parameters

```js
app.get("/search", (req, res) => {
  res.send(req.query.q);
});
```

---

## 8. Express vs Node.js (Direct Comparison)

| Feature      | Node.js | Express.js |
| ------------ | ------- | ---------- |
| Type         | Runtime | Framework  |
| Routing      | Manual  | Built-in   |
| Middleware   | No      | Yes        |
| Code size    | Large   | Small      |
| API creation | Hard    | Easy       |
| Scalability  | Low     | High       |

---

## 9. When to use Express?

Use Express when:

* Building APIs
* Building backend for React / Angular / Vue
* Creating microservices
* Handling authentication and middleware

Do not use only Node.js for large apps.

---

## 10. Interview One-Line Summary

* **Node.js** provides the runtime
* **Express.js** provides structure and simplicity
* Express makes backend development faster and cleaner


