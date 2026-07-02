# REST API - Android Pentesting

## What is a REST API?

**REST (Representational State Transfer)** is an **architectural style** for designing web services that allows clients and servers to communicate over HTTP/HTTPS.

An **API (Application Programming Interface)** is a set of rules that allows different software applications to communicate with each other.

A **REST API** enables applications (such as Android apps) to send requests to a server and receive responses.

Example:

```text
Android App
      │
HTTPS Request
      │
      ▼
REST API
      │
Process Request
      │
      ▼
Database
      │
      ▼
REST API
      │
HTTPS Response
      │
      ▼
Android App
```

---

# Why are REST APIs Important?

Understanding REST APIs helps you:

- Analyze Android app communication
- Test mobile applications
- Find API vulnerabilities
- Perform authorization testing
- Understand backend functionality
- Conduct Android penetration testing

Most modern Android applications communicate with backend servers using REST APIs.

---

# Why Do We Need APIs?

Imagine an Android banking app.

Instead of directly accessing the database, the app communicates with a REST API.

```text
Android App
      │
      ▼
REST API
      │
      ▼
Database
```

Benefits:

- Better security
- Easier maintenance
- Scalability
- Controlled access

---

# REST Architecture

A REST API sits between the client and the server resources.

```text
Android App
      │
HTTPS
      │
      ▼
REST API
      │
      ▼
Database
```

---

# REST Principles

REST APIs follow several architectural principles.

## 1. Client-Server

The client and server are separate.

```text
Android App
      │
      ▼
REST API
```

The client requests data.

The server processes the request.

---

## 2. Stateless

Each request is independent.

The server does **not** remember previous requests.

Every request should include everything needed to process it.

Example:

```http
GET /profile
Authorization: Bearer TOKEN
```

If the token is missing:

```http
401 Unauthorized
```

---

## 3. Uniform Interface

REST APIs use consistent URLs and HTTP methods.

Example:

```text
GET /users

POST /users

DELETE /users/10
```

---

## 4. Resource-Based

Everything is treated as a resource.

Examples:

```text
/users

/products

/orders

/messages
```

---

# Resources

A resource is any object managed by the API.

Examples:

```text
User

Product

Order

Invoice

Message
```

Each resource usually has its own endpoint.

---

# Endpoint

An endpoint is the URL used to access a resource.

Example:

```text
https://api.example.com/users
```

More examples:

```text
/users

/orders

/login

/profile
```

---

# HTTP Methods in REST

## GET

Retrieve data.

Example:

```http
GET /users
```

---

## POST

Create new data.

Example:

```http
POST /users
```

Body:

```json
{
  "name": "Usman"
}
```

---

## PUT

Replace an existing resource.

Example:

```http
PUT /users/5
```

---

## PATCH

Update part of a resource.

Example:

```http
PATCH /users/5
```

---

## DELETE

Delete a resource.

Example:

```http
DELETE /users/5
```

---

# CRUD Operations

| Operation | HTTP Method |
|-----------|-------------|
| Create | POST |
| Read | GET |
| Update | PUT / PATCH |
| Delete | DELETE |

---

# REST API URL Structure

Example:

```text
https://api.example.com/users/15
```

Components:

```text
Protocol : https
Host     : api.example.com
Resource : users
ID       : 15
```

---

# Path Parameters

Used to identify a specific resource.

Example:

```text
/users/10
```

Here:

```text
10
```

is the path parameter.

---

# Query Parameters

Used to filter or search resources.

Example:

```text
GET /users?page=2
```

Multiple parameters:

```text
GET /users?page=2&role=admin
```

---

# Request Headers

Headers provide additional information.

Common headers:

```http
Authorization: Bearer TOKEN

Content-Type: application/json

Accept: application/json
```

---

# Request Body

POST, PUT, and PATCH usually include a request body.

Example:

```json
{
  "username": "admin",
  "password": "password123"
}
```

---

# Response Body

Example:

```json
{
  "id": 10,
  "username": "usman",
  "role": "user"
}
```

Most REST APIs use **JSON**.

---

# JSON

JSON (JavaScript Object Notation) is the most common data format used by REST APIs.

Example:

```json
{
  "id": 10,
  "name": "Ali",
  "email": "ali@example.com"
}
```

---

# HTTP Status Codes

## Success

| Code | Meaning |
|------|---------|
|200|OK|
|201|Created|
|204|No Content|

---

## Client Errors

| Code | Meaning |
|------|---------|
|400|Bad Request|
|401|Unauthorized|
|403|Forbidden|
|404|Not Found|
|409|Conflict|
|429|Too Many Requests|

---

## Server Errors

| Code | Meaning |
|------|---------|
|500|Internal Server Error|
|502|Bad Gateway|
|503|Service Unavailable|

---

# Authentication

REST APIs commonly use:

- JWT Tokens
- OAuth
- Session Cookies
- API Keys
- Basic Authentication

Example:

```http
Authorization: Bearer eyJhb...
```

---

# REST API Request Example

```http
POST /api/login HTTP/1.1
Host: api.example.com
Content-Type: application/json

{
    "username":"admin",
    "password":"password123"
}
```

---

# REST API Response Example

```http
HTTP/1.1 200 OK

{
    "token":"eyJhb..."
}
```

---

# REST API Lifecycle

```text
Android App
      │
Create HTTP Request
      │
      ▼
REST API
      │
Authenticate
      │
Validate Input
      │
Database Query
      │
Generate Response
      │
      ▼
Android App
```

---

# REST API from an Android App

```text
User Clicks Login
      │
      ▼
Android App
      │
POST /login
      │
      ▼
REST API
      │
Check Credentials
      │
Generate JWT
      │
      ▼
Response
```

---

# Android Pentesting Perspective

Most Android applications expose REST API endpoints.

Examples:

```text
/api/login

/api/profile

/api/orders

/api/messages

/api/payments
```

During testing, inspect:

- Authentication
- Authorization
- Request Headers
- Request Body
- Response Body
- HTTP Methods
- User IDs
- Object IDs
- Error Messages

---

# Common REST API Vulnerabilities

- Broken Authentication
- Broken Authorization
- Broken Object Level Authorization (BOLA / IDOR)
- Broken Function Level Authorization (BFLA)
- Mass Assignment
- Excessive Data Exposure
- Security Misconfiguration
- SQL Injection
- NoSQL Injection
- Command Injection
- Server-Side Request Forgery (SSRF)
- Insecure File Upload
- Rate Limiting Issues

---

# REST API Security Best Practices

- Always use HTTPS.
- Validate all user input on the server.
- Enforce authentication and authorization for every request.
- Implement rate limiting.
- Return only the data required by the client.
- Use proper HTTP status codes.
- Log security-relevant events.
- Do not expose sensitive information in error messages.

---

# Common Tools

| Tool | Purpose |
|------|----------|
| Burp Suite | Intercept and modify API requests |
| Postman | Test REST APIs |
| curl | Send HTTP requests from CLI |
| ADB | Interact with Android devices |
| Frida | Runtime instrumentation |
| Objection | Mobile security testing |
| Wireshark | Capture network traffic |

---

# Useful Commands

GET request:

```bash
curl https://api.example.com/users
```

POST request:

```bash
curl -X POST https://api.example.com/login \
-H "Content-Type: application/json" \
-d '{"username":"admin","password":"password"}'
```

PUT request:

```bash
curl -X PUT https://api.example.com/users/10 \
-H "Content-Type: application/json" \
-d '{"name":"Usman"}'
```

DELETE request:

```bash
curl -X DELETE https://api.example.com/users/10
```

---

# Example Android Pentesting Workflow

```text
Install APK
      │
      ▼
Configure Burp Proxy
      │
      ▼
Launch App
      │
      ▼
Capture REST API Requests
      │
      ▼
Analyze Endpoints
      │
      ▼
Modify Parameters
      │
      ▼
Replay Requests
      │
      ▼
Test Authentication & Authorization
      │
      ▼
Identify Vulnerabilities
```

---

# REST vs SOAP

| REST | SOAP |
|------|------|
| Architectural style | Protocol |
| Usually uses JSON | Usually uses XML |
| Lightweight | More verbose |
| Faster | Generally slower |
| Widely used in mobile apps | Common in some enterprise systems |

---

# REST vs GraphQL

| REST | GraphQL |
|------|----------|
| Multiple endpoints | Usually a single endpoint |
| Fixed response structure | Client specifies required fields |
| Simpler to understand | More flexible |
| Widely adopted | Increasingly popular |

---

# Best Practices

- Learn common REST API endpoints and HTTP methods.
- Understand the difference between path parameters and query parameters.
- Inspect every request and response during testing.
- Verify authentication and authorization on every endpoint.
- Test object identifiers (`user_id`, `account_id`, `order_id`) for authorization flaws.
- Analyze error messages for information disclosure.

---

# Key Takeaways

- **REST API** is an architectural style that enables communication between clients and servers using HTTP/HTTPS.
- REST APIs are **resource-based**, **stateless**, and use standard HTTP methods such as **GET**, **POST**, **PUT**, **PATCH**, and **DELETE**.
- Most Android applications communicate with backend services using REST APIs and exchange data in **JSON** format.
- Android penetration testers should analyze endpoints, HTTP methods, authentication, authorization, request parameters, and responses to identify vulnerabilities such as **BOLA/IDOR**, **Broken Authentication**, **Mass Assignment**, and **Security Misconfigurations**.
