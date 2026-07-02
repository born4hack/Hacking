# Digital Certificates - Android Pentesting

## What is a Digital Certificate?

A **Digital Certificate** is an electronic document that verifies the identity of a website, server, application, or user.

It binds a **public key** to an identity and is used by **TLS/HTTPS** to establish secure communication.

Think of it as a **digital ID card** for a server.

Example:

```text
Server
   │
   ▼
Digital Certificate
   │
   ▼
"I am api.example.com"
```

---

# Why are Certificates Important?

Certificates help you:

- Verify the identity of servers
- Prevent Man-in-the-Middle (MITM) attacks
- Enable HTTPS
- Encrypt communications
- Authenticate websites and APIs
- Secure Android applications

Without certificates, anyone could pretend to be the server you're trying to connect to.

---

# Why Do We Need Certificates?

Imagine logging into a banking app.

Without certificates:

```text
Android App
      │
      ▼
"Trust me, I'm your bank!"
      │
      ▼
Attacker
```

The app has no reliable way to know if it's talking to the real bank.

With certificates:

```text
Android App
      │
Verify Certificate
      │
      ▼
Real Bank Server
```

The app verifies the server's identity before sending sensitive data.

---

# What Information Does a Certificate Contain?

A certificate typically contains:

- Domain name
- Public key
- Certificate owner (Subject)
- Certificate issuer
- Serial number
- Validity period
- Digital signature
- Signature algorithm
- Public key algorithm

Example:

```text
Subject:
api.example.com

Issuer:
Let's Encrypt

Valid From:
2026-01-01

Expires:
2027-01-01
```

---

# Public Key and Private Key

Certificates use **Public Key Cryptography**.

```text
Public Key
      │
Encrypt / Verify
      │
      ▼
Private Key
      │
Decrypt / Sign
```

- **Public Key** → Shared with everyone.
- **Private Key** → Kept secret by the owner.

The certificate contains **only the public key**, never the private key.

---

# How Certificates Work

```text
Android App
      │
Connect to Server
      │
      ▼
Server Sends Certificate
      │
      ▼
App Verifies Certificate
      │
      ▼
Certificate Valid?
   │            │
 Yes            No
 │              │
 ▼              ▼
TLS Starts   Connection Fails
```

---

# Certificate Validation

Before trusting a certificate, the client checks:

- Is the certificate expired?
- Is it signed by a trusted Certificate Authority (CA)?
- Does the domain name match?
- Has the certificate been revoked?
- Is the certificate chain valid?

If any check fails, the TLS connection is usually rejected.

---

# Certificate Chain

Certificates are trusted through a **chain of trust**.

```text
Root CA
    │
    ▼
Intermediate CA
    │
    ▼
Server Certificate
```

The client verifies each certificate in the chain until it reaches a trusted Root CA.

---

# Certificate Authority (CA)

A **Certificate Authority (CA)** is a trusted organization that issues digital certificates.

Examples:

- Let's Encrypt
- DigiCert
- Sectigo
- GlobalSign
- Google Trust Services

Browsers and Android devices trust certificates issued by recognized CAs.

---

# Self-Signed Certificate

A **Self-Signed Certificate** is signed by its own creator instead of a trusted CA.

Example:

```text
Server
   │
Signs Its Own Certificate
```

Advantages:

- Free
- Easy to create
- Useful for development and testing

Disadvantages:

- Not trusted by default
- Causes browser or app security warnings

---

# Root Certificate

A **Root Certificate** belongs to a trusted Root Certificate Authority.

```text
Root CA
    │
Trusted by Operating System
```

Operating systems and browsers include trusted Root CA certificates in their trust stores.

---

# Intermediate Certificate

Instead of signing every server certificate directly, Root CAs issue **Intermediate Certificates**.

```text
Root CA
     │
Intermediate CA
     │
Server Certificate
```

This improves security by protecting the Root CA's private key.

---

# Server Certificate

A **Server Certificate** identifies a specific server.

Example:

```text
api.example.com
```

When an Android app connects to this server, it receives the server certificate during the TLS handshake.

---

# Wildcard Certificate

A **Wildcard Certificate** secures multiple subdomains.

Example:

```text
*.example.com
```

It can secure:

```text
api.example.com

mail.example.com

login.example.com
```

It does **not** secure:

```text
example.com
```

unless the certificate explicitly includes it.

---

# SAN (Subject Alternative Name)

A certificate can protect multiple domains using **Subject Alternative Names (SANs)**.

Example:

```text
example.com

www.example.com

api.example.com

mail.example.com
```

All can be covered by a single certificate.

---

# Certificate Formats

Common certificate file formats:

| Format | Description |
|---------|-------------|
| `.pem` | Base64 encoded certificate |
| `.crt` | Certificate file |
| `.cer` | Certificate file |
| `.der` | Binary certificate |
| `.p7b` | Certificate chain |
| `.p12` / `.pfx` | Certificate + Private Key |

---

# PEM Example

```text
-----BEGIN CERTIFICATE-----

MIID...

-----END CERTIFICATE-----
```

---

# Certificate Lifecycle

```text
Generate Key Pair
       │
       ▼
Create CSR
       │
       ▼
Certificate Authority
       │
       ▼
Certificate Issued
       │
       ▼
Install Certificate
       │
       ▼
Use HTTPS
       │
       ▼
Renew Before Expiration
```

---

# Certificate Signing Request (CSR)

A **Certificate Signing Request (CSR)** is sent to a Certificate Authority to request a certificate.

A CSR contains:

- Public key
- Organization details
- Domain name

The private key **never leaves** the owner.

---

# Certificate Expiration

Certificates have a limited validity period.

Example:

```text
Valid From:
2026

Expires:
2027
```

Expired certificates are generally rejected by clients.

---

# Certificate Revocation

Sometimes certificates must be revoked before they expire.

Reasons:

- Private key compromised
- Certificate issued incorrectly
- Organization ownership changes

Clients can check revocation using:

- CRL (Certificate Revocation List)
- OCSP (Online Certificate Status Protocol)

---

# Android Certificate Store

Android devices maintain a **trusted certificate store** containing Root CA certificates.

```text
Android
     │
Trusted Root Certificates
```

Apps typically trust these certificates unless they implement a custom trust configuration.

---

# Certificate Pinning

Some Android apps implement **Certificate Pinning**.

Instead of trusting every system CA, the app trusts only a specific certificate or public key.

Example:

```text
Android App
      │
      ▼
Only Trust This Certificate
      │
      ▼
Server
```

Benefits:

- Reduces the risk of certain MITM attacks.

Challenges for pentesters:

- Burp Suite interception may fail.
- Frida or Objection is commonly used to bypass pinning during authorized security assessments.

---

# Android Pentesting Perspective

During Android application testing, inspect:

- Certificate validity
- Expiration date
- Certificate issuer
- Domain matching
- Certificate chain
- Certificate pinning
- Custom trust managers
- Self-signed certificates

---

# Common Certificate Vulnerabilities

- Improper Certificate Validation
- Accepting Any Certificate
- Trusting Self-Signed Certificates Insecurely
- Missing Hostname Verification
- Weak Certificate Pinning
- Expired Certificates
- Misconfigured Trust Managers
- Leaked Private Keys

---

# Common Tools

| Tool | Purpose |
|------|----------|
| Burp Suite | HTTPS interception |
| OpenSSL | Inspect certificates |
| keytool | Manage Java keystores |
| Frida | Bypass certificate pinning (authorized testing) |
| Objection | Mobile security testing |
| Wireshark | Analyze TLS traffic |
| ADB | Android device interaction |

---

# Useful Commands

View a server certificate:

```bash
openssl s_client -connect example.com:443
```

Show certificate chain:

```bash
openssl s_client -connect example.com:443 -showcerts
```

View certificate details:

```bash
openssl x509 -in certificate.pem -text -noout
```

View a Java keystore:

```bash
keytool -list -v -keystore keystore.jks
```

---

# Example Android Pentesting Workflow

```text
Install APK
      │
      ▼
Launch App
      │
      ▼
Capture HTTPS Traffic
      │
      ▼
Inspect Certificate
      │
      ▼
Check Issuer
      │
      ▼
Check Expiration
      │
      ▼
Check Certificate Pinning
      │
      ▼
Test HTTPS Interception
```

---

# Best Practices

- Always obtain certificates from trusted Certificate Authorities.
- Renew certificates before they expire.
- Protect private keys and never share them.
- Verify certificate chains and hostname validation.
- Implement certificate pinning where appropriate.
- Avoid disabling certificate validation in production applications.

---

# Key Takeaways

- A **Digital Certificate** verifies the identity of a server, application, or user.
- Certificates contain a **public key**, identity information, validity period, and a **digital signature** from a trusted Certificate Authority.
- During the TLS handshake, clients validate certificates before establishing encrypted communication.
- Android applications rely on certificates to secure HTTPS connections and verify server identities.
- Android penetration testers should examine certificate validation, trust chains, expiration, and certificate pinning to identify security weaknesses.
