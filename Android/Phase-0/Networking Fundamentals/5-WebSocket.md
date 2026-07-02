# WebSocket - Android Pentesting

## What is WebSocket?

**WebSocket** is a **communication protocol** that provides a **persistent, full-duplex (two-way)** connection between a client and a server.

Unlike HTTP, where a new request/response cycle occurs for each interaction, WebSocket keeps a single connection open, allowing both the client and the server to send data at any time.

Example:

```text
Android App
      │
WebSocket Connection
══════════════════════
      │
      ▼
WebSocket Server
```

---

# Why is WebSocket Important?

Understanding WebSocket helps you:

- Analyze real-time communication
- Test mobile applications using WebSockets
- Discover authorization vulnerabilities
- Find IDORs
- Test chat applications
- Test live notifications
- Test multiplayer games
- Test financial and trading apps

Many modern Android applications use WebSockets for real-time communication.

---

# Why Not Just Use HTTP?

HTTP follows a **Request → Response** model.

Example:

```text
Client
   │
HTTP Request
   │
   ▼
Server
   │
HTTP Response
   │
   ▼
Client
```

If updates are needed continuously, the client must repeatedly send requests (polling).

Example:

```text
Request
Response

Request
Response

Request
Response
```

This wastes:

- Bandwidth
- CPU
- Battery
- Network resources

---

# WebSocket Solution

WebSocket establishes **one connection** and keeps it open.

```text
Client
    ═══════════════════
          WebSocket
    ═══════════════════
Server

Both can send data anytime.
```

No repeated HTTP requests are required after the connection is established.

---

# HTTP vs WebSocket

| HTTP | WebSocket |
|------|-----------|
| Request → Response | Full-duplex communication |
| New connection for each request (HTTP/1.1) | Single persistent connection |
| Client initiates communication | Client and server can both send messages |
| Higher overhead for frequent updates | Low overhead after connection setup |
| Best for REST APIs | Best for real-time applications |

---

# Where WebSockets are Used

Common examples:

- Chat applications
- Live notifications
- Multiplayer games
- Stock market apps
- Live sports scores
- Cryptocurrency exchanges
- Ride-sharing apps
- Real-time dashboards
- Collaborative editors

Android examples:

- WhatsApp
- Discord
- Slack
- Telegram
- Trading apps

---

# How WebSocket Works

## Step 1 — HTTP Handshake

A WebSocket connection starts as a normal HTTP request.

```text
Android App
      │
HTTP Upgrade Request
      │
      ▼
Server
```

Example request:

```http
GET /chat HTTP/1.1
Host: example.com
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: abc123
Sec-WebSocket-Version: 13
```

---

## Step 2 — Server Response

If accepted:

```http
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
```

Status code:

```text
101 Switching Protocols
```

The connection is now a WebSocket connection.

---

## Step 3 — Persistent Connection

```text
Client
═══════════════════════
      WebSocket
═══════════════════════
Server
```

Messages can now flow in both directions.

---

# Full-Duplex Communication

Both sides can send data whenever needed.

```text
Client  ─────────► Server

Client ◄───────── Server
```

Unlike HTTP, the server does not have to wait for a client request before sending data.

---

# WebSocket URL

Instead of `http://` or `https://`, WebSocket uses:

Unencrypted:

```text
ws://example.com/chat
```

Encrypted:

```text
wss://example.com/chat
```

---

# ws vs wss

| Protocol | Description |
|----------|-------------|
| `ws://` | Unencrypted WebSocket |
| `wss://` | Encrypted WebSocket using TLS |

Always prefer **`wss://`** for production applications.

---

# WebSocket Messages

Messages may contain:

- JSON
- Text
- Binary data

JSON Example:

```json
{
  "action": "send_message",
  "message": "Hello"
}
```

---

# Message Flow

```text
Android App
      │
Connect
      │
      ▼
Server
      │
Connection Open
      │
◄══════════════►
Real-Time Messages
```

---

# WebSocket Frames

Unlike HTTP requests and responses, WebSockets exchange **frames**.

Common frame types:

- Text Frame
- Binary Frame
- Ping Frame
- Pong Frame
- Close Frame

Example:

```text
Text Frame

{
    "message":"Hello"
}
```

---

# WebSocket Lifecycle

```text
Client
   │
HTTP Upgrade
   │
   ▼
101 Switching Protocols
   │
   ▼
Connection Open
   │
   ▼
Exchange Messages
   │
   ▼
Connection Closed
```

---

# Common WebSocket Events

| Event | Description |
|--------|-------------|
| Open | Connection established |
| Message | Data received |
| Error | Communication error |
| Close | Connection terminated |

---

# WebSocket Authentication

Authentication may occur using:

- JWT Tokens
- Session Cookies
- OAuth Tokens
- API Keys
- Custom Tokens

Examples:

Header:

```http
Authorization: Bearer eyJhb...
```

Cookie:

```http
Cookie: session=abc123
```

Query parameter (less secure):

```text
wss://example.com/chat?token=abc123
```

---

# Common Android WebSocket Usage

Real-time chat:

```text
User Sends Message
      │
      ▼
WebSocket
      │
      ▼
Server
      │
      ▼
Other Users Instantly Receive Message
```

Notifications:

```text
Server
      │
Push Event
      │
      ▼
Android App
```

---

# Android Pentesting Perspective

Many Android apps use WebSockets for:

- Chat
- Notifications
- Live locations
- Trading
- Gaming
- Device synchronization

During testing, inspect:

- Authentication
- Authorization
- User IDs
- Organization IDs
- Room IDs
- Message IDs
- Hidden API actions

---

# Common WebSocket Vulnerabilities

- Broken Authentication
- Broken Authorization
- IDOR
- Privilege Escalation
- Parameter Tampering
- Information Disclosure
- Missing Access Control
- Session Hijacking
- Insecure Direct Object References
- Business Logic Flaws

Example:

```json
{
  "user_id": 1001
}
```

Changing it to:

```json
{
  "user_id": 1002
}
```

If another user's data is returned, it may indicate an **IDOR** vulnerability.

---

# Intercepting WebSockets

Unlike REST APIs, WebSocket messages continue after the connection is established.

Burp Suite allows you to:

- Intercept WebSocket traffic
- Modify messages
- Replay messages
- Fuzz parameters
- Inspect server responses

---

# Useful Tools

| Tool | Purpose |
|------|----------|
| Burp Suite | Intercept WebSocket traffic |
| Chrome DevTools | Inspect WebSocket messages |
| Wireshark | Packet capture |
| Frida | Runtime instrumentation |
| Objection | Mobile testing |
| ADB | Android device interaction |
| websocat | Command-line WebSocket client |
| wscat | Command-line WebSocket client |

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
Identify WebSocket Connection
      │
      ▼
Capture Messages
      │
      ▼
Modify Parameters
      │
      ▼
Replay Messages
      │
      ▼
Test Authorization
      │
      ▼
Identify Vulnerabilities
```

---

# Best Practices

- Always use **`wss://`** instead of **`ws://`** in production.
- Authenticate users before allowing WebSocket communication.
- Perform authorization checks on **every** message, not just when the connection is established.
- Validate all incoming message data on the server.
- Never trust client-supplied identifiers such as `user_id` or `room_id`.
- Monitor WebSocket traffic during mobile application security testing.

---

# Key Takeaways

- **WebSocket** is a protocol that enables **persistent, full-duplex communication** between clients and servers.
- A WebSocket connection starts with an **HTTP Upgrade** request and then switches to the WebSocket protocol.
- Unlike HTTP, WebSockets allow both the client and the server to send messages at any time over a single connection.
- WebSockets are widely used in Android apps for chat, notifications, games, live tracking, and financial applications.
- Android penetration testers should inspect WebSocket authentication, authorization, message contents, and identifiers to identify vulnerabilities such as **IDOR**, **Broken Access Control**, and **Privilege Escalation**.
