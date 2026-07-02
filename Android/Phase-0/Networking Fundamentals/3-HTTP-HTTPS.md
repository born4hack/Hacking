# HTTP & HTTPS - Android Pentesting

## What is HTTP?

**HTTP (HyperText Transfer Protocol)** is an **application-layer protocol** used to transfer data between a **client** and a **server**.

It is the foundation of communication on the World Wide Web.

Example:

```text
Android App / Browser
          │
     HTTP Request
          │
          ▼
       Web Server
          │
     HTTP Response
          │
          ▼
Android App / Browser
```

---

# Why are HTTP and HTTPS Important?

Understanding HTTP and HTTPS helps you:

- Analyze Android app traffic
- Test APIs
- Identify security vulnerabilities
- Intercept requests and responses
- Understand authentication mechanisms
- Perform mobile application penetration testing

Almost every Android application communicates with a backend server using **HTTP** or **HTTPS**.

---

# What is HTTPS?

**HTTPS (HyperText Transfer Protocol Secure)** is the secure version of HTTP.

It uses **TLS (Transport Layer Security)** to encrypt communication between the client and the server.

Example:

```text
Android App
      │
Encrypted HTTPS Request
      │
      ▼
Web Server
      │
Encrypted HTTPS Response
      │
      ▼
Android App
```

---

# HTTP vs HTTPS

| HTTP | HTTPS |
|------|-------|
| No encryption | Encrypted using TLS |
| Data can be intercepted | Data is encrypted in transit |
| Uses Port 80 | Uses Port 443 |
| Vulnerable to MITM attacks | Protects against eavesdropping and tampering |
| URL starts with `http://` | URL starts with `https://` |

---

# Client-Server Communication

HTTP follows the **Client-Server** model.

```text
Client
(Android App)
      │
HTTP Request
      │
      ▼
Server
(API Server)
      │
HTTP Response
      │
      ▼
Client
```

Examples of clients:

- Android Apps
- Web Browsers
- Postman
- curl

Examples of servers:

- API Server
- Web Server
- Authentication Server

---

# HTTP Request

An HTTP request is sent from the client to the server.

It consists of:

- Request Line
- Headers
- Optional Body

Example:

```http
GET /api/profile HTTP/1.1
Host: example.com
Authorization: Bearer TOKEN
User-Agent: Android

```

Structure:

```text
Request Line
Headers
Blank Line
Body (Optional)
```

---

# HTTP Response

The server responds with:

- Status Line
- Headers
- Optional Body

Example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "username": "usman"
}
```

Structure:

```text
Status Line
Headers
Blank Line
Response Body
```

---

# HTTP Methods

| Method | Purpose |
|---------|----------|
| GET | Retrieve data |
| POST | Create new data |
| PUT | Replace existing data |
| PATCH | Update part of existing data |
| DELETE | Delete data |
| HEAD | Retrieve headers only |
| OPTIONS | List supported HTTP methods |

Examples:

Retrieve user profile:

```http
GET /api/profile
```

Login:

```http
POST /api/login
```

Update email:

```http
PATCH /api/user
```

Delete account:

```http
DELETE /api/user
```

---

# HTTP Status Codes

## 1xx — Informational

| Code | Meaning |
|------|---------|
|100|Continue|
|101|Switching Protocols|

---

## 2xx — Success

| Code | Meaning |
|------|---------|
|200|OK|
|201|Created|
|202|Accepted|
|204|No Content|

---

## 3xx — Redirection

| Code | Meaning |
|------|---------|
|301|Moved Permanently|
|302|Found|
|304|Not Modified|

---

## 4xx — Client Errors

| Code | Meaning |
|------|---------|
|400|Bad Request|
|401|Unauthorized|
|403|Forbidden|
|404|Not Found|
|405|Method Not Allowed|
|429|Too Many Requests|

---

## 5xx — Server Errors

| Code | Meaning |
|------|---------|
|500|Internal Server Error|
|502|Bad Gateway|
|503|Service Unavailable|
|504|Gateway Timeout|

---

# Common HTTP Headers

## Request Headers

| Header | Purpose |
|---------|----------|
|Host|Target server|
|User-Agent|Client information|
|Authorization|Authentication token|
|Accept|Accepted response types|
|Content-Type|Type of request body|
|Cookie|Session information|
|Referer|Previous page|
|Origin|Origin of the request|

Example:

```http
Authorization: Bearer eyJhb...
```

---

## Response Headers

| Header | Purpose |
|---------|----------|
|Content-Type|Response format|
|Content-Length|Response size|
|Server|Server software|
|Set-Cookie|Creates cookies|
|Cache-Control|Caching policy|
|Location|Redirect URL|

---

# URL Structure

Example:

```text
https://api.example.com:443/users?id=10
```

Components:

```text
Protocol : https
Host     : api.example.com
Port     : 443
Path     : /users
Query    : id=10
```

---

# Query Parameters

Used to send data in the URL.

Example:

```text
GET /users?id=10
```

Multiple parameters:

```text
GET /users?id=10&role=admin
```

Android Pentesting:

Common vulnerabilities:

- IDOR
- Parameter Tampering
- SQL Injection
- Information Disclosure

---

# Request Body

Used mainly with:

- POST
- PUT
- PATCH

JSON Example:

```json
{
  "username": "admin",
  "password": "password123"
}
```

Common Content Types:

| Content-Type | Description |
|--------------|-------------|
|application/json|JSON data|
|application/xml|XML|
|application/x-www-form-urlencoded|HTML form data|
|multipart/form-data|File uploads|
|text/plain|Plain text|

---

# Cookies

Cookies store session information.

Example:

```http
Cookie: session=abc123
```

Response:

```http
Set-Cookie: session=abc123
```

Android apps may use:

- Cookies
- JWT Tokens
- OAuth Tokens

---

# Authentication

Common authentication methods:

- Basic Authentication
- Bearer Token (JWT)
- OAuth
- API Keys
- Session Cookies

Example:

```http
Authorization: Bearer eyJhb...
```

---

# TLS Handshake (HTTPS)

Before encrypted communication starts:

```text
Client
   │
Client Hello
   │
   ▼
Server
   │
Server Hello
Certificate
   │
   ▼
Key Exchange
   │
   ▼
Encrypted Communication
```

TLS provides:

- Encryption
- Integrity
- Authentication

---

# HTTP Request Lifecycle

```text
Android App
      │
Create Request
      │
DNS Lookup
      │
TCP Connection
      │
TLS Handshake (HTTPS)
      │
HTTP Request
      │
Server Processing
      │
HTTP Response
      │
Android App
```

---

# HTTP/1.1 vs HTTP/2 vs HTTP/3

| Feature | HTTP/1.1 | HTTP/2 | HTTP/3 |
|----------|----------|---------|---------|
|Transport|TCP|TCP|QUIC (UDP-based)|
|Multiplexing|No|Yes|Yes|
|Header Compression|No|Yes|Yes|
|Performance|Good|Better|Best|

Most modern Android apps support **HTTP/2**, and some also support **HTTP/3** depending on the client library and server.

---

# Android Pentesting Perspective

During mobile application testing, you'll inspect:

- API requests
- API responses
- JWT Tokens
- Authentication
- Authorization
- Cookies
- File uploads
- HTTP Headers
- JSON Bodies
- Error messages

Common vulnerabilities:

- IDOR
- Broken Authentication
- Missing Authorization
- Sensitive Data Exposure
- Insecure Direct Object References
- SQL Injection
- Command Injection
- XXE (XML APIs)
- Rate Limiting Issues

---

# Common Tools

| Tool | Purpose |
|------|----------|
|Burp Suite|Intercept and modify HTTP/HTTPS traffic|
|Postman|API testing|
|curl|Send HTTP requests from CLI|
|Wireshark|Packet capture|
|Frida|Runtime instrumentation|
|Objection|Mobile security testing|
|ADB|Interact with Android devices|

---

# Useful Commands

Send a GET request:

```bash
curl https://example.com
```

View only headers:

```bash
curl -I https://example.com
```

Send a POST request:

```bash
curl -X POST https://example.com/api/login \
-H "Content-Type: application/json" \
-d '{"username":"admin","password":"password"}'
```

Download a file:

```bash
curl -O https://example.com/file.apk
```

---

# Example Android Pentesting Workflow

```text
Install APK
      │
      ▼
Configure Proxy
      │
      ▼
Launch App
      │
      ▼
Capture HTTPS Requests
      │
      ▼
Analyze Headers
      │
      ▼
Modify Parameters
      │
      ▼
Test Authentication
      │
      ▼
Identify Vulnerabilities
```

---

# Best Practices

- Always use HTTPS instead of HTTP for sensitive data.
- Verify TLS certificates.
- Avoid transmitting sensitive information in URLs.
- Use secure authentication mechanisms such as OAuth or JWT.
- Validate all user input on the server.
- Inspect every request and response during penetration testing.
- Understand how headers, cookies, and tokens are used by the application.

---

# Key Takeaways

- **HTTP** is an application-layer protocol used for communication between clients and servers.
- **HTTPS** is the secure version of HTTP, providing encryption through **TLS**.
- HTTP communication consists of **requests** and **responses**.
- Understanding HTTP methods, status codes, headers, cookies, and authentication is essential for Android penetration testing.
- Android pentesters use tools like **Burp Suite**, **Postman**, **curl**, **Wireshark**, and **ADB** to inspect, intercept, and modify HTTP/HTTPS traffic to identify security vulnerabilities.
