# TCP/IP Model - Android Pentesting

## What is the TCP/IP Model?

The **TCP/IP (Transmission Control Protocol / Internet Protocol) Model** is the practical networking model used by the Internet.

It describes how data is transmitted between devices across networks.

Unlike the **OSI Model** (which has 7 layers), the **TCP/IP Model** has **4 layers** and is used in real-world networking.

Example:

```text
Application (Android App)
          │
          ▼
Application Layer
          │
          ▼
Transport Layer
          │
          ▼
Internet Layer
          │
          ▼
Network Access Layer
          │
          ▼
Wi-Fi / Ethernet
          │
          ▼
Internet
```

---

# Why is the TCP/IP Model Important?

Understanding the TCP/IP model helps you:

- Understand how the Internet works
- Analyze Android app traffic
- Capture and inspect network packets
- Troubleshoot networking problems
- Perform network and mobile penetration testing
- Understand where protocols like HTTP, TCP, and IP operate

For Android Pentesting, nearly all communication between an app and a server follows the TCP/IP model.

---

# The 4 Layers of the TCP/IP Model

| Layer | Responsibility |
|--------|----------------|
| Application | User applications and network services |
| Transport | End-to-end communication |
| Internet | Logical addressing and routing |
| Network Access | Physical network communication |

---

# Layer 4 — Application Layer

The **Application Layer** is where users and applications communicate over the network.

It combines the responsibilities of the **Application**, **Presentation**, and **Session** layers from the OSI model.

Examples:

- Android apps
- Web browsers
- Email clients
- API clients

Common Protocols:

- HTTP
- HTTPS
- DNS
- FTP
- SMTP
- SSH
- WebSocket

Example:

```text
Android App
      │
      ▼
HTTPS API Request
```

Android Pentesting:

You'll frequently analyze:

- REST APIs
- GraphQL APIs
- Authentication
- Authorization
- JWT Tokens
- Cookies
- WebSockets

Useful tools:

- Burp Suite
- Postman
- curl
- Frida
- Objection

---

# Layer 3 — Transport Layer

Responsible for:

- Reliable communication
- Data segmentation
- Flow control
- Error detection
- Port numbers

Main Protocols:

- TCP
- UDP

Example:

```text
Large Data
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
| 53 | DNS (also uses UDP) |

Android Pentesting:

You'll inspect:

- TCP connections
- UDP traffic
- Open ports
- Network sockets

Useful commands:

```bash
ss -tuln
```

```bash
netstat -tuln
```

Useful tools:

- Wireshark
- tcpdump
- Nmap
- netcat (`nc`)

---

# Layer 2 — Internet Layer

Responsible for:

- Logical addressing
- Routing packets
- Selecting the best path to the destination

Protocols:

- IPv4
- IPv6
- ICMP
- IGMP

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
- ICMP (Ping)

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

# Layer 1 — Network Access Layer

Responsible for:

- Sending data over the physical network
- Framing
- MAC addressing
- Error detection on the local network

Technologies:

- Ethernet
- Wi-Fi
- Fiber
- USB Networking

Example:

```text
Laptop
   │
MAC Address
   │
Wi-Fi Router
   │
Android Phone
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

- ARP Spoofing
- Local Network Enumeration
- Man-in-the-Middle (MITM) Attacks
- Wi-Fi Analysis

---

# Data Flow in TCP/IP

```text
Application
      │
Transport
      │
Internet
      │
Network Access
      │
~~~~~~~~~~~~~~~
Internet
~~~~~~~~~~~~~~~
      │
Network Access
      │
Internet
      │
Transport
      │
Application
```

---

# Encapsulation

As data moves down the TCP/IP model, each layer adds its own header.

```text
Application Data
        │
        ▼
TCP Header
        │
        ▼
IP Header
        │
        ▼
Ethernet/Wi-Fi Header
        │
        ▼
Transmit
```

When the destination receives the data, each layer removes its corresponding header. This is called **Decapsulation**.

---

# Example Android Network Request

```text
User Logs In
      │
      ▼
Android App
      │
      ▼
HTTPS POST Request
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

# TCP/IP Model from an Android Pentester's Perspective

| Layer | Android Pentesting Example |
|--------|----------------------------|
| Application | API Testing, Authentication, IDOR, WebSockets |
| Transport | TCP/UDP Traffic, Open Ports |
| Internet | IP Routing, VPN, DNS Resolution |
| Network Access | Wi-Fi, Ethernet, ARP, MITM |

---

# Common Protocols by Layer

| Layer | Common Protocols |
|--------|------------------|
| Application | HTTP, HTTPS, DNS, FTP, SSH, SMTP, WebSocket |
| Transport | TCP, UDP |
| Internet | IPv4, IPv6, ICMP, IGMP |
| Network Access | Ethernet, Wi-Fi (802.11), ARP |

---

# Common Tools by Layer

| Tool | Primary Layer(s) |
|------|------------------|
| Burp Suite | Application |
| Postman | Application |
| curl | Application |
| Frida | Application |
| Objection | Application |
| Wireshark | All Layers |
| tcpdump | Network Access, Internet, Transport |
| Nmap | Internet, Transport |
| netcat (`nc`) | Transport |
| ADB | Application (Android interaction) |

---

# TCP vs UDP

| TCP | UDP |
|-----|-----|
| Connection-oriented | Connectionless |
| Reliable | Faster but less reliable |
| Guarantees delivery | No delivery guarantee |
| Error checking | Minimal error checking |
| Used for web browsing | Used for streaming, VoIP, gaming |

Examples:

TCP:

- HTTPS
- SSH
- FTP

UDP:

- DNS Queries
- VoIP
- Online Gaming
- Video Streaming

---

# OSI vs TCP/IP

| OSI Model | TCP/IP Model |
|-----------|--------------|
| 7 Layers | 4 Layers |
| Conceptual model | Practical model |
| Used for learning | Used by the Internet |

Mapping:

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

# Android Pentesting Workflow

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
Inspect Traffic
      │
      ▼
Modify Requests
      │
      ▼
Test for Vulnerabilities
```

---

# Common Commands

Check IP addresses:

```bash
ip addr
```

View routing table:

```bash
ip route
```

Check active connections:

```bash
ss -tuln
```

Capture packets:

```bash
sudo tcpdump -i wlan0
```

Resolve DNS:

```bash
dig google.com
```

Test connectivity:

```bash
ping google.com
```

Scan open ports:

```bash
nmap 192.168.1.10
```

---

# Best Practices

- Understand the responsibility of each TCP/IP layer rather than memorizing them.
- Learn where common protocols like HTTP, HTTPS, TCP, UDP, and IP operate.
- Use Wireshark or tcpdump to observe packets at different layers.
- Practice intercepting Android app traffic with Burp Suite.
- Relate every network vulnerability to the TCP/IP layer it affects.

---

# Key Takeaways

- The **TCP/IP Model** is the practical networking model used by the Internet.
- It consists of **4 layers**: **Application**, **Transport**, **Internet**, and **Network Access**.
- Android applications communicate with servers using protocols defined in the TCP/IP model.
- Understanding this model is essential for Android penetration testing, as it helps analyze traffic, troubleshoot networking issues, and identify vulnerabilities.
- Tools like **Burp Suite**, **Wireshark**, **tcpdump**, **Nmap**, **ADB**, and **Frida** help analyze and test different layers of the TCP/IP model.
