# OSI Model - Android Pentesting

## What is the OSI Model?

The **OSI (Open Systems Interconnection) Model** is a **7-layer conceptual framework** that explains how data travels from one device to another over a network.

Each layer has a specific responsibility and communicates with the layers directly above and below it.

Example:

```text
Application (You)
        │
        ▼
Layer 7 - Application
        │
        ▼
Layer 6 - Presentation
        │
        ▼
Layer 5 - Session
        │
        ▼
Layer 4 - Transport
        │
        ▼
Layer 3 - Network
        │
        ▼
Layer 2 - Data Link
        │
        ▼
Layer 1 - Physical
        │
        ▼
Network Cable / Wi-Fi
```

---

# Why is the OSI Model Important?

Understanding the OSI model helps you:

- Understand how networks work
- Troubleshoot network issues
- Analyze application traffic
- Understand where attacks occur
- Perform network and mobile penetration testing
- Use tools like Wireshark, Burp Suite, and tcpdump effectively

For Android Pentesting, almost every network communication follows this layered model.

---

# The 7 Layers of the OSI Model

| Layer | Name | Main Responsibility |
|--------|------|---------------------|
| 7 | Application | User-facing network services |
| 6 | Presentation | Data formatting, encryption, compression |
| 5 | Session | Managing communication sessions |
| 4 | Transport | Reliable data delivery |
| 3 | Network | Routing packets between networks |
| 2 | Data Link | Communication within the local network |
| 1 | Physical | Sending bits over physical media |

---

# Layer 7 — Application Layer

The **Application Layer** is where users and applications interact with the network.

Examples:

- Web browser
- Mobile apps
- Email client
- API clients

Common protocols:

- HTTP
- HTTPS
- FTP
- SMTP
- DNS

Android examples:

- Banking app
- WhatsApp
- Instagram
- Browser

Example:

```text
Android App
      │
      ▼
HTTPS Request
```

As a pentester, you'll often test this layer using:

- Burp Suite
- Postman
- curl

---

# Layer 6 — Presentation Layer

Responsible for:

- Encryption
- Compression
- Data formatting
- Encoding

Examples:

- TLS/SSL encryption
- JSON
- XML
- Base64
- JPEG
- PNG

Example:

```text
Plain Text
     │
     ▼
Encrypt (TLS)
     │
     ▼
Encrypted Data
```

Android Pentesting:

You'll frequently encounter:

- TLS certificates
- Certificate pinning
- JWT tokens
- JSON APIs

---

# Layer 5 — Session Layer

Responsible for:

- Starting sessions
- Maintaining sessions
- Ending sessions

Examples:

- User login session
- API authentication session
- WebSocket connection

Example:

```text
Login
   │
   ▼
Session Created
   │
   ▼
User Requests
   │
   ▼
Logout
```

Android Pentesting:

You'll test:

- Session management
- Session expiration
- Session hijacking
- Token reuse

---

# Layer 4 — Transport Layer

Responsible for:

- Reliable communication
- Data segmentation
- Error checking
- Port numbers

Protocols:

- TCP
- UDP

Example:

```text
Large File
      │
      ▼
Split into Segments
      │
      ▼
Transmit
```

Common Ports:

| Port | Service |
|------|---------|
| 80 | HTTP |
| 443 | HTTPS |
| 22 | SSH |
| 21 | FTP |
| 53 | DNS |

Android Pentesting:

You'll inspect:

- TCP traffic
- UDP traffic
- Open ports
- Network sockets

Useful tools:

- Wireshark
- tcpdump
- netstat
- ss

---

# Layer 3 — Network Layer

Responsible for:

- Logical addressing
- Routing
- Packet forwarding

Protocol:

- IP (IPv4 / IPv6)

Example:

```text
Source IP
     │
     ▼
Router
     │
     ▼
Destination IP
```

Android Pentesting:

You'll encounter:

- IP addresses
- VPNs
- Routing
- Packet captures

Useful commands:

```bash
ip addr
```

```bash
ip route
```

```bash
ping google.com
```

---

# Layer 2 — Data Link Layer

Responsible for:

- Communication on the local network
- MAC addressing
- Ethernet frames

Protocols:

- Ethernet
- Wi-Fi (802.11)

Example:

```text
Laptop
   │
 MAC Address
   │
Switch
   │
Phone
```

Useful commands:

```bash
ip link
```

```bash
arp -a
```

Android Pentesting:

Understanding this layer helps with:

- ARP spoofing
- MITM attacks
- Local network analysis

---

# Layer 1 — Physical Layer

Responsible for transmitting raw bits through:

- Ethernet cable
- Wi-Fi radio
- Fiber optics
- USB

Example:

```text
0 1 0 1 1 0 0 1
```

Examples:

- Wi-Fi signals
- Network cables
- USB cables

Although pentesters rarely attack this layer, it's the foundation for all network communication.

---

# OSI Data Flow

```text
Application
      │
Presentation
      │
Session
      │
Transport
      │
Network
      │
Data Link
      │
Physical
      │
~~~~~~~~~~~~~
Internet
~~~~~~~~~~~~~
      │
Physical
      │
Data Link
      │
Network
      │
Transport
      │
Session
      │
Presentation
      │
Application
```

---

# Encapsulation

As data moves down the OSI model, each layer adds its own information (header). This process is called **Encapsulation**.

```text
Application Data
        │
        ▼
+ Application Data +
        │
        ▼
+ Transport Header +
        │
        ▼
+ IP Header +
        │
        ▼
+ Ethernet Header +
        │
        ▼
Transmit
```

When received, the headers are removed. This process is called **Decapsulation**.

---

# OSI Model from an Android App

```text
Android App
      │
HTTPS Request
      │
TLS Encryption
      │
TCP
      │
IP
      │
Wi-Fi
      │
Internet
```

---

# Android Pentesting Perspective

| OSI Layer | Android Pentesting Example |
|-----------|----------------------------|
| Application | API testing, IDOR, Authentication |
| Presentation | TLS, Certificate Pinning, JWT |
| Session | Session Tokens, Cookies |
| Transport | TCP, UDP, Open Ports |
| Network | IP Routing, VPN, DNS |
| Data Link | ARP, Wi-Fi, MITM |
| Physical | USB Debugging, Physical Device Access |

---

# Common Tools by Layer

| Tool | Primary Layer(s) |
|------|------------------|
| Burp Suite | 7, 6 |
| Wireshark | 2–7 |
| tcpdump | 2–4 |
| ADB | 7 (interaction with Android), also supports debugging over USB/TCP |
| Frida | 7 |
| Objection | 7 |
| Nmap | 3–4 |
| netcat (`nc`) | 4 |
| curl | 7 |

---

# Example Android Network Request

```text
User taps Login
        │
        ▼
Android App
        │
        ▼
HTTPS POST
        │
        ▼
TLS Encryption
        │
        ▼
TCP Segment
        │
        ▼
IP Packet
        │
        ▼
Wi-Fi Frame
        │
        ▼
Internet
        │
        ▼
Server
```

---

# OSI vs TCP/IP Model

| OSI Model | TCP/IP Model |
|-----------|--------------|
| 7 Layers | 4 Layers |
| Conceptual model | Practical implementation |
| Used for learning and troubleshooting | Used by the Internet |

TCP/IP mapping:

| OSI Layer | TCP/IP Layer |
|-----------|--------------|
| Application | Application |
| Presentation | Application |
| Session | Application |
| Transport | Transport |
| Network | Internet |
| Data Link | Network Access |
| Physical | Network Access |

---

# OSI Model in Android Pentesting Workflow

```text
Android App
      │
      ▼
HTTPS Request
      │
      ▼
Burp Suite
      │
      ▼
Analyze Traffic
      │
      ▼
Modify Requests
      │
      ▼
Identify Vulnerabilities
```

---

# Common Commands

Check network interfaces:

```bash
ip addr
```

Check routing table:

```bash
ip route
```

Check open connections:

```bash
ss -tuln
```

Capture packets:

```bash
sudo tcpdump -i wlan0
```

Test connectivity:

```bash
ping google.com
```

Resolve DNS:

```bash
dig google.com
```

---

# Best Practices

- Learn the responsibility of each OSI layer instead of just memorizing them.
- Understand where common protocols (HTTP, HTTPS, TCP, IP, DNS) fit in the model.
- Use packet capture tools like Wireshark and tcpdump to observe traffic at different layers.
- Practice intercepting Android app traffic with Burp Suite to see the Application and Presentation layers in action.
- Relate every vulnerability you find to the OSI layer it affects.

---

# Memory Trick

Remember the layers from top to bottom:

```text
All
People
Seem
To
Need
Data
Processing
```

Or from bottom to top:

```text
Please
Do
Not
Throw
Sausage
Pizza
Away
```

---

# Key Takeaways

- The **OSI Model** is a conceptual framework that explains how network communication works using **7 layers**.
- Each layer has a specific responsibility, from user applications down to physical transmission.
- Android applications primarily operate at the **Application Layer**, while data passes through every layer before reaching the server.
- Understanding the OSI model helps Android penetration testers analyze traffic, troubleshoot networking issues, intercept requests, and identify vulnerabilities.
- Tools like **Burp Suite**, **Wireshark**, **tcpdump**, **ADB**, and **Frida** operate at different layers of the OSI model, making it essential knowledge for mobile security testing.
