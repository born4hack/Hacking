# Android Interface Definition Language (AIDL)

## Definition

**AIDL (Android Interface Definition Language)** is a language used in Android to define the interface between two processes that communicate using **Binder IPC**.

It allows one process to call methods that are implemented in another process, as if they were local function calls.

Think of AIDL as a **contract** that both the client and the server agree to follow.

Example:

```text
Client App

↓

AIDL Interface

↓

Binder IPC

↓

Remote Service
```

Without AIDL, Android developers would have to manually implement complex Binder communication.

---

# Why Does Android Need AIDL?

Android applications usually run in separate Linux processes.

Example:

```text
Music App

PID 2100
```

```text
Media Service

PID 4150
```

Since they run in different processes:

```text
Music App

✗ Cannot directly call

Media Service functions
```

Instead:

```text
Music App

↓

AIDL Interface

↓

Binder IPC

↓

Media Service

↓

Response
```

AIDL makes this communication simple and structured.

---

# What Problem Does AIDL Solve?

Imagine a music application wants to play a song.

Without AIDL:

```text
Music App

↓

Manually Serialize Data

↓

Binder

↓

Deserialize Data

↓

Execute Function
```

Developers would need to write a large amount of Binder-related code.

With AIDL:

```text
Music App

↓

playSong()

↓

Binder Handles Communication

↓

Music Starts
```

The complexity is hidden from the developer.

---

# Where Does AIDL Fit?

AIDL is built on top of Binder IPC.

```text
Application

↓

AIDL

↓

Binder IPC

↓

Remote Service
```

Binder transports the data, while AIDL defines **what data and methods can be exchanged**.

---

# How AIDL Works

The communication process follows these steps.

```text
Client

↓

Call AIDL Method

↓

Binder Driver

↓

Remote Service

↓

Execute Method

↓

Return Result
```

Although the method executes in another process, it appears like a normal function call.

---

# Components of AIDL

AIDL communication consists of several components.

```text
Client

↓

Proxy

↓

Binder Driver

↓

Stub

↓

Service
```

Each component has a specific role.

---

# Step 1 — AIDL Interface

The developer first creates an AIDL file.

Example:

```text
IMusicService.aidl
```

The interface defines the methods that clients can call.

Example:

```text
Play Song

Pause Song

Stop Song

Get Current Song
```

Think of the interface as a list of allowed remote functions.

---

# Step 2 — Proxy (Client Side)

Android automatically generates a **Proxy** class for the client.

Example:

```text
Music App

↓

Proxy

↓

Binder
```

The Proxy:

- Receives the method call
- Packages the data
- Sends it through Binder

The client does not communicate with Binder directly.

---

# Step 3 — Binder Driver

The Binder Driver transfers the request between processes.

Example:

```text
Proxy

↓

Binder Driver

↓

Stub
```

The Binder Driver:

- Transfers data
- Performs context switching
- Validates communication

---

# Step 4 — Stub (Server Side)

Android automatically generates a **Stub** class.

Example:

```text
Binder Driver

↓

Stub

↓

Music Service
```

The Stub:

- Receives Binder data
- Unpacks the request
- Calls the actual service method

---

# Step 5 — Remote Service

Finally, the actual service executes the requested method.

Example:

```text
Music Service

↓

Play Song

↓

Return Success
```

The response then travels back through Binder to the client.

---

# Complete Communication Flow

```text
Music App

↓

Proxy

↓

Binder Driver

↓

Stub

↓

Music Service

↓

Result

↓

Binder Driver

↓

Music App
```

---

# Real Example — Playing Music

Suppose a music application wants to play a song.

```text
Music App

↓

playSong()

↓

Proxy

↓

Binder

↓

Music Service

↓

Play Audio

↓

Return Success
```

To the developer, it looks like a simple function call.

---

# Real Example — GPS Service

```text
Maps App

↓

getCurrentLocation()

↓

AIDL

↓

Location Service

↓

GPS Hardware

↓

Coordinates Returned
```

---

# Real Example — Calculator Service

Imagine a calculator service running in another process.

```text
Calculator App

↓

add(5,3)

↓

AIDL

↓

Calculator Service

↓

Return 8
```

Although the calculation happens in another process, it behaves like a local method call.

---

# What Can AIDL Transfer?

AIDL supports many data types.

Examples include:

- int
- long
- float
- boolean
- String
- Arrays
- Lists
- Maps
- Parcelable Objects

Example:

```text
Client

↓

User Object

↓

Binder

↓

Service
```

Complex objects are usually transferred using **Parcelable**.

---

# Parcelable

Since Binder cannot directly transfer complex Java objects, Android uses **Parcelable**.

Example:

```text
User

├── Name

├── Age

└── Email
```

Parcelable converts the object into a format that Binder can transfer efficiently.

---

# AIDL and Permissions

AIDL itself does **not** provide security.

The service receiving the request must verify permissions.

Example:

```text
Client

↓

Binder Request

↓

Service

↓

Permission Check

↓

Allowed?
```

If permissions are missing:

```text
Request

↓

Denied
```

---

# AIDL vs Binder

| Feature | Binder | AIDL |
|----------|---------|------|
| What is it? | IPC mechanism | Interface definition language |
| Runs in Kernel | Yes (Binder Driver) | No |
| Defines Methods | No | Yes |
| Transfers Data | Yes | Uses Binder |
| Generates Code | No | Yes |

Think of it this way:

```text
Binder

=

Road

AIDL

=

Map describing where you can travel
```

---

# AIDL from a Pentester's Perspective

Many Android applications expose Binder services using AIDL.

Security topics include:

- Exported AIDL services
- Missing permission checks
- Insecure IPC
- Information disclosure
- Privilege escalation
- Unauthorized method invocation
- Parcelable deserialization issues
- Service enumeration

Applications should never assume that only trusted apps will call their AIDL methods.

---

# Real-World Example

Suppose a banking application exposes an AIDL service.

```text
Banking App

↓

AIDL Service

↓

transferMoney()
```

A legitimate client:

```text
Official Banking App

↓

transferMoney()

↓

Permission Check

↓

Transfer Complete
```

A malicious application:

```text
Malicious App

↓

transferMoney()

↓

Permission Check Missing

↓

Money Transferred
```

If the service does not properly validate the caller or enforce permissions, unauthorized applications may invoke sensitive methods, leading to privilege escalation or unauthorized actions.

---

# Key Terms

| Term | Meaning |
|------|---------|
| AIDL | Android Interface Definition Language |
| Binder | Android's IPC mechanism used to transfer data between processes |
| Proxy | Client-side class generated from the AIDL interface |
| Stub | Server-side class generated from the AIDL interface |
| Remote Service | Service that implements the AIDL methods |
| Parcelable | Android interface for efficiently serializing complex objects |
| Binder Driver | Linux kernel driver that transfers Binder messages |
| IPC | Inter-Process Communication |
| Transaction | A request and response exchanged through Binder |

---

# Key Takeaways

- AIDL is a language used to define interfaces for communication between Android processes.
- It is built on top of Binder IPC and hides the complexity of low-level Binder communication.
- Android automatically generates Proxy and Stub classes from AIDL definitions.
- Clients invoke remote methods through the Proxy, while the Stub receives the requests and calls the actual service implementation.
- Binder transports the data, while AIDL defines the methods and data types that can be exchanged.
- Complex objects are transferred using the Parcelable mechanism.
- Improperly secured AIDL services can expose sensitive functionality, making them an important target during Android penetration testing.
