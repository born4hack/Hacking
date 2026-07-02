# TLS (Transport Layer Security) - Android Pentesting

## What is TLS?

**TLS (Transport Layer Security)** is a **cryptographic protocol** that provides **secure communication** over a network.

It encrypts data exchanged between a client and a server, preventing attackers from reading or modifying the communication.

TLS is the technology that makes **HTTPS** secure.

Example:

```text
Android App
      │
Encrypted TLS Connection
      │
      ▼
API Server
```

Without TLS, data would travel in plaintext and could be intercepted.

---

# Why is TLS Important?

Understanding TLS helps you:

- Understand how HTTPS works
- Analyze secure Android app communication
- Test certificate validation
- Understand certificate pinning
- Configure Burp Suite for HTTPS interception
- Identify TLS-related vulnerabilities

Almost every Android application communicates with backend servers using TLS.

---

# Why Do We Need TLS?

Imagine logging into a banking application.

Without TLS:

```text
Android App
      │
username=admin
password=123456
      │
      ▼
Internet
```

Anyone monitoring the network could read the credentials.

With TLS:

```text
Android App
      │
Encrypted Data
      │
      ▼
Internet
```

Attackers see encrypted data instead of the actual username and password.

---

# What Does TLS Provide?

TLS provides three main security properties:

| Property | Description |
|----------|-------------|
| Confidentiality | Encrypts data so only authorized parties can read it |
| Integrity | Detects if data has been modified during transmission |
| Authentication | Verifies the identity of the server (and optionally the client) |

---

# TLS vs SSL

| SSL | TLS |
|-----|-----|
| Older protocol | Modern replacement |
| No longer considered secure | Secure and actively maintained |
| SSL 2.0 & SSL 3.0 are deprecated | TLS 1.2 & TLS 1.3 are recommended |

Today, people often say **"SSL certificate"**, but modern HTTPS actually uses **TLS**.

---

# Where TLS is Used

TLS protects many Internet protocols, including:

- HTTPS
- FTPS
- SMTPS
- IMAPS
- POP3S
- LDAPS
- MQTT over TLS
- Secure WebSockets (`wss://`)

---

# How TLS Works

```text
Android App
      │
TCP Connection
      │
      ▼
TLS Handshake
      │
      ▼
Encrypted Session
      │
      ▼
HTTPS Requests
      │
      ▼
API Server
```

---

# TLS Handshake

Before encrypted communication begins, the client and server perform a **TLS Handshake**.

Simplified process:

```text
Client
   │
ClientHello
   │
   ▼
Server
   │
ServerHello
Certificate
   │
   ▼
Key Exchange
   │
   ▼
Session Keys Created
   │
   ▼
Encrypted Communication
```

---

# Step 1 — ClientHello

The client sends:

- Supported TLS versions
- Supported cipher suites
- Random value
- Extensions (such as SNI)

Example:

```text
ClientHello
TLS 1.3
Supported Ciphers
Random Value
```

---

# Step 2 — ServerHello

The server replies with:

- Selected TLS version
- Selected cipher suite
- Random value
- Digital certificate

---

# Step 3 — Certificate Verification

The client verifies the server certificate.

Checks include:

- Is the certificate valid?
- Is it expired?
- Is it signed by a trusted Certificate Authority (CA)?
- Does the domain name match the certificate?

If validation fails, the connection is usually terminated.

---

# Step 4 — Key Exchange

The client and server establish a shared secret used to generate symmetric session keys.

Modern TLS commonly uses **ECDHE (Elliptic Curve Diffie-Hellman Ephemeral)** for secure key exchange.

---

# Step 5 — Secure Communication

After the handshake:

```text
HTTP
     │
TLS Encryption
     │
     ▼
HTTPS
```

All application data is encrypted using the negotiated session keys.

---

# Symmetric vs Asymmetric Encryption

## Symmetric Encryption

One shared key is used for both encryption and decryption.

Examples:

- AES
- ChaCha20

Advantages:

- Fast
- Efficient
- Used after the TLS handshake

---

## Asymmetric Encryption

Uses a **public key** and a **private key**.

Examples:

- RSA
- ECC

Advantages:

- Secure key exchange
- Digital signatures

TLS uses asymmetric cryptography during the handshake and symmetric encryption for the data transfer.

---

# Digital Certificates

A **digital certificate** proves the identity of the server.

It contains:

- Domain name
- Public key
- Issuer (Certificate Authority)
- Validity period
- Digital signature

Example:

```text
Domain:
api.example.com

Issued By:
Let's Encrypt

Expires:
2027
```

---

# Certificate Authorities (CA)

A **Certificate Authority (CA)** is a trusted organization that issues digital certificates.

Examples:

- Let's Encrypt
- DigiCert
- GlobalSign
- Sectigo

Browsers and Android trust certificates issued by recognized CAs.

---

# Certificate Chain

```text
Root CA
    │
Intermediate CA
    │
Server Certificate
```

The client verifies the chain before trusting the server.

---

# Cipher Suites

A **Cipher Suite** defines the algorithms used during a TLS connection.

It specifies:

- Key exchange
- Authentication
- Encryption
- Message integrity

Example:

```text
TLS_AES_128_GCM_SHA256
```

---

# TLS Versions

| Version | Status |
|----------|--------|
| SSL 2.0 | Deprecated |
| SSL 3.0 | Deprecated |
| TLS 1.0 | Deprecated |
| TLS 1.1 | Deprecated |
| TLS 1.2 | Widely Supported |
| TLS 1.3 | Recommended |

Modern Android applications should use **TLS 1.2** or **TLS 1.3**.

---

# TLS in HTTPS

```text
Android App
      │
HTTPS Request
      │
TLS Encryption
      │
      ▼
Internet
      │
      ▼
API Server
```

Without TLS:

```text
HTTP
```

With TLS:

```text
HTTPS
```

---

# Certificate Pinning

Some Android applications implement **Certificate Pinning**.

Instead of trusting all system Certificate Authorities, the app trusts only specific certificates or public keys.

Benefits:

- Helps protect against some Man-in-the-Middle (MITM) attacks, even if a rogue or compromised CA issues a certificate.

Challenges for pentesters:

- Burp Suite interception may fail.
- Frida or Objection is often used to bypass pinning in authorized security assessments.

---

# Android Pentesting Perspective

During Android penetration testing, inspect:

- TLS versions
- Certificate validation
- Certificate pinning
- Weak cipher suites
- Insecure trust configurations
- Self-signed certificates
- Expired certificates

---

# Common TLS Vulnerabilities

- Improper Certificate Validation
- Missing Certificate Validation
- Weak TLS Versions
- Weak Cipher Suites
- Trusting User-Added Certificates Insecurely
- Broken Certificate Pinning
- Expired Certificates
- Misconfigured Trust Managers

---

# Common Tools

| Tool | Purpose |
|------|----------|
| Burp Suite | HTTPS interception |
| Wireshark | Analyze TLS traffic |
| OpenSSL | Inspect TLS connections |
| Frida | Bypass certificate pinning (authorized testing) |
| Objection | Mobile security testing |
| ADB | Android device interaction |

---

# Useful Commands

View certificate information:

```bash
openssl s_client -connect example.com:443
```

Show certificate details:

```bash
openssl s_client -connect example.com:443 -showcerts
```

Retrieve only headers:

```bash
curl -I https://example.com
```

Test supported TLS versions:

```bash
nmap --script ssl-enum-ciphers example.com
```

---

# Example Android Pentesting Workflow

```text
Install APK
      │
      ▼
Configure Burp Certificate
      │
      ▼
Launch App
      │
      ▼
Observe TLS Handshake
      │
      ▼
Check Certificate Validation
      │
      ▼
Identify Certificate Pinning
      │
      ▼
Intercept HTTPS Traffic
      │
      ▼
Analyze Requests & Responses
```

---

# Best Practices

- Use **TLS 1.2** or **TLS 1.3**.
- Disable deprecated SSL and TLS versions.
- Validate server certificates correctly.
- Use strong cipher suites.
- Implement certificate pinning where appropriate.
- Regularly renew certificates before expiration.
- Avoid custom TrustManagers that bypass certificate validation.

---

# Key Takeaways

- **TLS (Transport Layer Security)** is the protocol that secures communication over the Internet.
- TLS provides **Confidentiality**, **Integrity**, and **Authentication**.
- HTTPS is simply **HTTP running over TLS**.
- The **TLS Handshake** establishes a secure session before encrypted communication begins.
- Android applications rely heavily on TLS for secure API communication.
- Android penetration testers should evaluate certificate validation, TLS versions, cipher suites, and certificate pinning to identify security weaknesses.
