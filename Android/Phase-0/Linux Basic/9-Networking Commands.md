# Linux Networking Commands - Android Pentesting

## What are Linux Networking Commands?

**Linux Networking Commands** are command-line utilities used to:

- View network information
- Test connectivity
- Troubleshoot network issues
- Monitor network traffic
- Discover hosts
- Inspect ports
- Analyze DNS
- Download resources

For Android Pentesting, these commands are essential for testing APIs, proxies, emulators, and device communication.

---

# Why are Networking Commands Important?

Networking commands help you:

- Verify internet connectivity
- Discover IP addresses
- Inspect routing
- Test DNS resolution
- Analyze network traffic
- Troubleshoot Android devices
- Test web servers and APIs

---

# Network Overview

```text
Android Device
        │
        ▼
Wi-Fi
        │
        ▼
Router
        │
        ▼
Internet
        │
        ▼
API Server
```

---

# ip

Displays and manages network interfaces, IP addresses, and routes.

View IP addresses:

```bash
ip addr
```

View interfaces:

```bash
ip link
```

View routing table:

```bash
ip route
```

Example output:

```text
2: wlan0
    inet 192.168.1.10/24
```

---

# ifconfig (Legacy)

Older command for viewing network interfaces.

```bash
ifconfig
```

Modern Linux systems prefer:

```bash
ip addr
```

---

# ping

Checks whether another host is reachable.

```bash
ping google.com
```

Example output:

```text
64 bytes from ...
```

Stop with:

```text
Ctrl + C
```

---

# traceroute

Shows the path packets take to reach a destination.

```bash
traceroute google.com
```

Example:

```text
Laptop
    ↓
Router
    ↓
ISP
    ↓
Google
```

---

# tracepath

A lightweight alternative to `traceroute`.

```bash
tracepath google.com
```

---

# nslookup

Queries DNS servers.

```bash
nslookup example.com
```

Example output:

```text
Address: 93.184.216.34
```

---

# dig

Advanced DNS lookup tool.

Query an A record:

```bash
dig example.com
```

Query an MX record:

```bash
dig example.com MX
```

Short output:

```bash
dig +short example.com
```

---

# host

Simple DNS lookup utility.

```bash
host example.com
```

---

# curl

Transfers data using HTTP, HTTPS, FTP, and many other protocols.

Download a webpage:

```bash
curl https://example.com
```

View only HTTP headers:

```bash
curl -I https://example.com
```

Send a POST request:

```bash
curl -X POST https://example.com/api
```

Send JSON:

```bash
curl -X POST \
-H "Content-Type: application/json" \
-d '{"username":"admin"}' \
https://example.com/api
```

---

# wget

Downloads files.

```bash
wget https://example.com/file.apk
```

Save with a custom name:

```bash
wget -O app.apk https://example.com/file.apk
```

---

# netstat (Legacy)

Displays network connections.

```bash
netstat -tuln
```

Shows:

- Listening ports
- TCP connections
- UDP connections

Modern replacement:

```bash
ss
```

---

# ss

Displays socket information.

Listening ports:

```bash
ss -tuln
```

Established connections:

```bash
ss -tun
```

---

# arp

Displays the ARP cache.

```bash
arp -a
```

Modern alternative:

```bash
ip neigh
```

---

# ip neigh

Displays neighboring devices on the local network.

```bash
ip neigh
```

---

# hostname

Display system hostname.

```bash
hostname
```

Display IP address:

```bash
hostname -I
```

---

# route (Legacy)

Displays routing table.

```bash
route
```

Modern replacement:

```bash
ip route
```

---

# nc (Netcat)

A powerful networking utility.

Connect to a TCP port:

```bash
nc example.com 80
```

Check if a port is open:

```bash
nc -zv example.com 443
```

---

# telnet

Tests TCP connectivity.

```bash
telnet example.com 80
```

Although still useful for testing, `nc` is generally preferred.

---

# openssl

Inspect SSL/TLS certificates.

```bash
openssl s_client -connect example.com:443
```

Useful for:

- Viewing certificates
- Testing TLS versions
- Checking certificate chains

---

# tcpdump

Captures network packets.

Capture traffic:

```bash
sudo tcpdump
```

Capture a specific interface:

```bash
sudo tcpdump -i wlan0
```

Save packets:

```bash
sudo tcpdump -w capture.pcap
```

Open the `.pcap` file in Wireshark for analysis.

---

# nmap

Scans hosts and ports.

Scan a host:

```bash
nmap 192.168.1.1
```

Service detection:

```bash
nmap -sV example.com
```

OS detection:

```bash
sudo nmap -O example.com
```

---

# Android Networking Commands

Open an ADB shell:

```bash
adb shell
```

View IP address:

```bash
ip addr
```

View routing table:

```bash
ip route
```

Ping a server:

```bash
ping google.com
```

View DNS properties:

```bash
getprop | grep dns
```

View network interfaces:

```bash
ip link
```

---

# ADB Port Forwarding

Forward a local port to the Android device:

```bash
adb forward tcp:8080 tcp:8080
```

Useful for:

- Frida
- Debugging
- Local testing

---

# Reverse Port Forwarding

Forward from the Android device back to the computer:

```bash
adb reverse tcp:8080 tcp:8080
```

Useful when testing apps running on emulators or physical devices.

---

# Networking Commands from a Pentester's Perspective

During Android penetration testing, these commands help you:

- Verify internet connectivity
- Test APIs
- Check DNS resolution
- Inspect SSL/TLS
- Scan hosts and ports
- Capture packets
- Debug proxy connections
- Configure ADB forwarding

---

# Example Android Pentesting Workflow

```text
Android App
      │
      ▼
HTTP Request
      │
      ▼
Burp Suite Proxy
      │
      ▼
API Server
```

Useful commands:

```bash
ping
curl
ss
tcpdump
adb forward
```

---

# Common Pentesting Commands

Check IP:

```bash
ip addr
```

Check routes:

```bash
ip route
```

Resolve DNS:

```bash
dig example.com
```

Test HTTP:

```bash
curl https://example.com
```

Scan ports:

```bash
nmap example.com
```

View listening ports:

```bash
ss -tuln
```

Capture traffic:

```bash
sudo tcpdump -i wlan0
```

Inspect certificates:

```bash
openssl s_client -connect example.com:443
```

---

# Command Summary

| Command | Purpose |
|----------|---------|
| `ip addr` | Show IP addresses |
| `ip route` | Show routing table |
| `ping` | Test connectivity |
| `traceroute` | Trace packet path |
| `dig` | Advanced DNS lookup |
| `nslookup` | Basic DNS lookup |
| `host` | DNS lookup |
| `curl` | Send HTTP/HTTPS requests |
| `wget` | Download files |
| `ss` | Show sockets and ports |
| `nc` | Connect to/test TCP ports |
| `openssl` | Inspect SSL/TLS certificates |
| `tcpdump` | Capture packets |
| `nmap` | Scan hosts and ports |
| `adb forward` | Forward local ports to Android |
| `adb reverse` | Forward Android ports to the host |

---

# Key Takeaways

- Linux networking commands are essential for inspecting, troubleshooting, and testing network communication.
- Modern Linux systems prefer **`ip`** over legacy commands like `ifconfig` and `route`, and **`ss`** over `netstat`.
- Android penetration testers frequently use tools such as **`curl`**, **`dig`**, **`tcpdump`**, **`nmap`**, and **ADB port forwarding** to analyze application traffic and network behavior.
- Mastering these commands is critical for API testing, proxy configuration, packet analysis, SSL/TLS validation, and mobile application security assessments.
