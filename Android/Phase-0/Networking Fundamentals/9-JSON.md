# JSON (JavaScript Object Notation) - Android Pentesting

## What is JSON?

**JSON (JavaScript Object Notation)** is a **lightweight, text-based data format** used to store and exchange data between applications.

Although it originated from JavaScript, JSON is **language-independent** and is supported by almost every programming language.

In Android Pentesting, you'll constantly see JSON in:

- API Requests
- API Responses
- Authentication Tokens
- Configuration Files
- Mobile App Data
- Firebase Responses

Example:

```json
{
    "username": "usman",
    "role": "user"
}
```

---

# Why is JSON Important?

Understanding JSON helps you:

- Read API requests
- Modify API responses
- Test Android applications
- Find API vulnerabilities
- Understand backend communication
- Use Burp Suite effectively

Almost every modern Android application communicates using JSON.

---

# Why Do We Need JSON?

Suppose an Android app asks a server for user information.

Instead of sending plain text:

```text
User: Usman
Age: 22
Role: User
```

The server sends structured JSON:

```json
{
    "name": "Usman",
    "age": 22,
    "role": "user"
}
```

JSON is:

- Easy for humans to read
- Easy for computers to parse
- Lightweight
- Platform-independent

---

# JSON Structure

A JSON document consists of:

- Objects
- Arrays
- Key-Value Pairs

Example:

```json
{
    "username": "admin",
    "email": "admin@example.com"
}
```

---

# JSON Object

An **Object** is enclosed in curly braces `{}`.

Example:

```json
{
    "name": "Ali",
    "age": 25
}
```

Structure:

```text
{
    key : value
}
```

---

# Key-Value Pair

Every piece of data is stored as:

```text
Key : Value
```

Example:

```json
{
    "username": "admin"
}
```

Here:

```text
Key   → username

Value → admin
```

---

# JSON Data Types

| Data Type | Example |
|-----------|----------|
| String | `"Usman"` |
| Number | `25` |
| Boolean | `true` |
| Null | `null` |
| Object | `{}` |
| Array | `[]` |

---

# String

Strings are enclosed in double quotes.

Example:

```json
{
    "city": "Lahore"
}
```

---

# Number

Numbers do not use quotes.

Example:

```json
{
    "age": 22
}
```

---

# Boolean

Boolean values are:

```json
true
false
```

Example:

```json
{
    "isAdmin": false
}
```

---

# Null

Represents an empty value.

Example:

```json
{
    "phone": null
}
```

---

# JSON Array

Arrays are enclosed in square brackets `[]`.

Example:

```json
{
    "roles": [
        "admin",
        "user",
        "manager"
    ]
}
```

---

# Array of Objects

Example:

```json
{
    "users": [
        {
            "id": 1,
            "name": "Ali"
        },
        {
            "id": 2,
            "name": "Usman"
        }
    ]
}
```

---

# Nested Objects

Objects can contain other objects.

Example:

```json
{
    "user": {
        "id": 1,
        "name": "Usman",
        "role": "admin"
    }
}
```

---

# Nested Arrays

Example:

```json
{
    "permissions": [
        "read",
        "write",
        "delete"
    ]
}
```

---

# Complete JSON Example

```json
{
    "id": 101,
    "username": "usman",
    "email": "usman@example.com",
    "isAdmin": false,
    "roles": [
        "user",
        "tester"
    ],
    "profile": {
        "city": "Lahore",
        "country": "Pakistan"
    }
}
```

---

# JSON in HTTP Request

Example:

```http
POST /api/login HTTP/1.1
Content-Type: application/json

{
    "username": "admin",
    "password": "password123"
}
```

---

# JSON in HTTP Response

Example:

```http
HTTP/1.1 200 OK

{
    "token": "eyJhbGci...",
    "userId": 1001
}
```

---

# JSON from an Android App

```text
Android App
      │
Create JSON
      │
      ▼
HTTPS Request
      │
      ▼
REST API
      │
      ▼
JSON Response
      │
      ▼
Android App
```

---

# Android Pentesting Perspective

During Android testing, you'll inspect JSON fields such as:

```json
{
    "user_id": 1001,
    "account_id": 500,
    "role": "user",
    "email": "user@example.com"
}
```

These fields often become attack targets.

---

# Common JSON Fields

Examples:

```json
"user_id"
```

```json
"account_id"
```

```json
"role"
```

```json
"isAdmin"
```

```json
"email"
```

```json
"price"
```

```json
"order_id"
```

---

# JSON Vulnerability Example (IDOR)

Original request:

```json
{
    "user_id": 1001
}
```

Modified request:

```json
{
    "user_id": 1002
}
```

If another user's data is returned, the application may have an **IDOR (Broken Object Level Authorization)** vulnerability.

---

# JSON Vulnerability Example (Privilege Escalation)

Original:

```json
{
    "role": "user"
}
```

Modified:

```json
{
    "role": "admin"
}
```

If accepted by the server, it indicates a serious authorization flaw.

---

# JSON Vulnerability Example (Price Manipulation)

Original:

```json
{
    "price": 100
}
```

Modified:

```json
{
    "price": 1
}
```

If the server trusts the client value, it may lead to a **Business Logic** vulnerability.

---

# JSON Validation

Servers should validate:

- Required fields
- Data types
- Input length
- Allowed values
- User permissions
- Business rules

Never trust client-supplied JSON.

---

# Pretty vs Minified JSON

Pretty JSON:

```json
{
    "name": "Ali",
    "age": 22
}
```

Minified JSON:

```json
{"name":"Ali","age":22}
```

Both contain the same data.

---

# Common JSON Errors

Missing quotes:

❌

```json
{
    name: "Ali"
}
```

Correct:

✅

```json
{
    "name": "Ali"
}
```

Trailing comma:

❌

```json
{
    "name": "Ali",
}
```

Correct:

✅

```json
{
    "name": "Ali"
}
```

---

# Common Tools

| Tool | Purpose |
|------|----------|
| Burp Suite | Modify JSON requests |
| Postman | Send JSON APIs |
| curl | Send JSON from CLI |
| jq | Parse and format JSON |
| Frida | Modify JSON at runtime |
| Objection | Mobile testing |
| ADB | Android device interaction |

---

# Useful Commands

Pretty-print JSON:

```bash
echo '{"name":"Ali","age":22}' | jq
```

Send JSON using curl:

```bash
curl -X POST https://api.example.com/login \
-H "Content-Type: application/json" \
-d '{"username":"admin","password":"password123"}'
```

Validate JSON:

```bash
cat data.json | jq .
```

---

# Example Android Pentesting Workflow

```text
Launch App
      │
      ▼
Capture API Request
      │
      ▼
Inspect JSON Body
      │
      ▼
Modify Fields
      │
      ▼
Replay Request
      │
      ▼
Analyze Response
      │
      ▼
Identify Vulnerabilities
```

---

# Best Practices

- Learn JSON syntax thoroughly.
- Inspect every JSON request and response during testing.
- Test object identifiers (`user_id`, `account_id`, `order_id`) for authorization flaws.
- Modify JSON values to test server-side validation.
- Never assume client-supplied JSON is trusted by the server.
- Use tools like Burp Suite and jq to inspect and manipulate JSON efficiently.

---

# Key Takeaways

- **JSON (JavaScript Object Notation)** is a lightweight, text-based format for exchanging structured data.
- JSON consists of **objects**, **arrays**, and **key-value pairs**.
- Most Android applications exchange data with REST APIs using JSON.
- Android penetration testers frequently modify JSON requests to test authentication, authorization, business logic, and input validation.
- Understanding JSON is essential for identifying vulnerabilities such as **IDOR (BOLA)**, **Privilege Escalation**, **Mass Assignment**, and **Business Logic Flaws**.
