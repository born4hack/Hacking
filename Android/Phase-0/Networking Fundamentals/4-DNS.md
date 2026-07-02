# DNS (Domain Name System) - Android Pentesting

## What is DNS?

**DNS (Domain Name System)** is the **phonebook of the Internet**.

Humans remember **domain names**, while computers communicate using **IP addresses**. DNS translates a **domain name** into an **IP address**.

Example:

```text
www.example.com
        │
        ▼
DNS Server
        │
        ▼
93.184.216.34
        │
        ▼
Connect to Server
```

Without DNS, you would have to remember IP addresses instead of domain names.

Example:

```text
Google
google.com
        │
        ▼
142.250.183.110
```

---

# Why is DNS Important?

Understanding DNS helps you:

- Understand how websites are located
- Analyze Android app network traffic
- Perform reconnaissance
- Enumerate subdomains
- Discover hidden services
- Troubleshoot network issues
- Identify security misconfigurations

Almost every Android application performs DNS lookups before communicating with its backend server.

---

# Why Do We Need DNS?

Imagine trying to visit Google.

Instead of typing:

```text
https://142.250.183.110
```

You simply type:

```text
https://google.com
```

DNS translates the domain into the correct IP address.

---

# How DNS Works

```text
User
 │
 ▼
google.com
 │
 ▼
DNS Resolver
 │
 ▼
Root DNS Server
 │
 ▼
TLD Server (.com)
 │
 ▼
Authoritative DNS Server
 │
 ▼
IP Address Returned
 │
 ▼
Browser / Android App
 │
 ▼
Connect to Server
```

---

# DNS Resolution Process

Suppose an Android app wants to connect to:

```text
api.example.com
```

Steps:

1. Android app requests `api.example.com`
2. Device asks the DNS resolver.
3. Resolver checks its cache.
4. If not cached, it queries the Root DNS server.
5. Root server points to the `.com` TLD server.
6. TLD server points to the authoritative DNS server.
7. Authoritative server returns the IP address.
8. The app connects to the returned IP address.

---

# Components of DNS

## Domain Name

Example:

```text
api.example.com
```

---

## IP Address

Example:

```text
192.168.1.100
```

or

```text
142.250.183.110
```

---

## DNS Resolver

Receives DNS queries from clients and finds the correct IP address.

Examples:

- ISP DNS
- Google DNS
- Cloudflare DNS

---

## Root DNS Server

Knows where the Top-Level Domain (TLD) servers are located.

Example:

```text
.com
.org
.net
```

---

## TLD Server

Handles top-level domains.

Examples:

```text
.com
.org
.edu
.gov
.io
.dev
```

---

## Authoritative DNS Server

Stores the official DNS records for a domain.

Example:

```text
example.com
```

---

# DNS Hierarchy

```text
               .
             (Root)
                │
      ┌─────────┴─────────┐
      │                   │
    .com                .org
      │
      ▼
example.com
      │
      ▼
api.example.com
```

---

# Common DNS Records

## A Record

Maps a domain name to an IPv4 address.

Example:

```text
example.com
      │
      ▼
192.168.1.20
```

Example record:

```text
example.com → 192.168.1.20
```

---

## AAAA Record

Maps a domain to an IPv6 address.

Example:

```text
example.com
      │
      ▼
2001:db8::1
```

---

## CNAME Record

Points one domain to another domain.

Example:

```text
www.example.com
        │
        ▼
example.com
```

---

## MX Record

Specifies the mail server for a domain.

Example:

```text
example.com
       │
       ▼
mail.example.com
```

Used for:

- Email delivery

---

## NS Record

Specifies the authoritative name servers for a domain.

Example:

```text
example.com
      │
      ▼
ns1.example.com
ns2.example.com
```

---

## TXT Record

Stores arbitrary text.

Common uses:

- Domain verification
- SPF
- DKIM
- DMARC

Example:

```text
v=spf1 include:_spf.google.com ~all
```

---

## PTR Record

Maps an IP address back to a domain name (Reverse DNS).

Example:

```text
192.168.1.20
      │
      ▼
example.com
```

---

# DNS Query Types

## Recursive Query

The DNS resolver finds the final answer for the client.

```text
Client
   │
   ▼
Resolver
   │
Find Everything
   │
   ▼
Return IP
```

---

## Iterative Query

The resolver receives referrals and continues querying other DNS servers.

---

# Forward DNS Lookup

```text
example.com
      │
      ▼
192.168.1.10
```

---

# Reverse DNS Lookup

```text
192.168.1.10
      │
      ▼
example.com
```

---

# DNS Caching

DNS results are cached to improve performance.

Example:

```text
Android App
      │
      ▼
DNS Cache
      │
Cached?
   │      │
 Yes      No
 │         │
 ▼         ▼
Use IP   Query DNS
```

Benefits:

- Faster responses
- Reduced network traffic
- Lower DNS server load

---

# DNS over HTTPS (DoH)

Traditional DNS queries are sent in plaintext.

**DNS over HTTPS (DoH)** encrypts DNS queries using HTTPS.

Benefits:

- Privacy
- Prevents DNS eavesdropping
- Helps protect against some DNS manipulation attacks

---

# DNS over TLS (DoT)

DNS over TLS also encrypts DNS traffic but uses a dedicated TLS connection instead of HTTPS.

---

# Common DNS Ports

| Protocol | Port |
|----------|------|
| DNS (UDP) | 53 |
| DNS (TCP) | 53 |
| DNS over TLS (DoT) | 853 |
| DNS over HTTPS (DoH) | 443 |

---

# Android Pentesting Perspective

Android applications often perform DNS lookups before sending API requests.

Example:

```text
Android App
      │
      ▼
api.bank.com
      │
      ▼
DNS Lookup
      │
      ▼
IP Address
      │
      ▼
HTTPS Connection
```

As a pentester, you'll inspect:

- API domains
- Subdomains
- Internal hostnames
- CDN endpoints
- Third-party services
- Cloud infrastructure

---

# DNS Reconnaissance

DNS information is valuable during reconnaissance.

Common activities:

- Subdomain Enumeration
- DNS Record Enumeration
- Reverse DNS Lookup
- Zone Transfer Testing (AXFR)
- CDN Identification
- Cloud Service Discovery

---

# Common DNS Attacks

- DNS Spoofing
- DNS Cache Poisoning
- DNS Hijacking
- Subdomain Takeover
- DNS Tunneling
- Misconfigured Zone Transfers
- Dangling DNS Records

---

# Useful Linux Commands

Lookup a domain:

```bash
dig example.com
```

Lookup an A record:

```bash
dig example.com A
```

Lookup an AAAA record:

```bash
dig example.com AAAA
```

Lookup MX records:

```bash
dig example.com MX
```

Lookup TXT records:

```bash
dig example.com TXT
```

Lookup NS records:

```bash
dig example.com NS
```

Reverse lookup:

```bash
dig -x 8.8.8.8
```

Use another DNS server:

```bash
dig @8.8.8.8 example.com
```

Using `host`:

```bash
host example.com
```

Using `nslookup`:

```bash
nslookup example.com
```

---

# Common Pentesting Tools

| Tool | Purpose |
|------|----------|
| dig | DNS lookups |
| nslookup | Query DNS records |
| host | Simple DNS queries |
| dnsrecon | DNS enumeration |
| dnsenum | DNS reconnaissance |
| amass | Subdomain enumeration |
| subfinder | Subdomain discovery |
| assetfinder | Passive subdomain enumeration |

---

# Example Android Pentesting Workflow

```text
Install APK
      │
      ▼
Launch App
      │
      ▼
Capture Traffic
      │
      ▼
Identify API Domain
      │
      ▼
Enumerate DNS Records
      │
      ▼
Discover Subdomains
      │
      ▼
Test Exposed Services
```

---

# Best Practices

- Understand how DNS translates domain names into IP addresses.
- Learn the purpose of common DNS records such as **A**, **AAAA**, **CNAME**, **MX**, **NS**, **TXT**, and **PTR**.
- Use tools like `dig`, `host`, and `nslookup` to inspect DNS records.
- During reconnaissance, enumerate subdomains and review DNS configurations for misconfigurations.
- Be aware that modern applications may use **DNS over HTTPS (DoH)** or **DNS over TLS (DoT)**, which encrypt DNS queries.

---

# Key Takeaways

- **DNS (Domain Name System)** translates domain names into IP addresses.
- DNS follows a hierarchical structure involving **Resolvers**, **Root Servers**, **TLD Servers**, and **Authoritative Name Servers**.
- Common DNS records include **A**, **AAAA**, **CNAME**, **MX**, **NS**, **TXT**, and **PTR**.
- Android applications perform DNS lookups before connecting to backend servers.
- Understanding DNS is essential for Android penetration testing, especially for reconnaissance, subdomain enumeration, traffic analysis, and identifying infrastructure-related vulnerabilities.
