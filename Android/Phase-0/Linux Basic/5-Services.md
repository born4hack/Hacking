# Linux Services - Android Pentesting

## What is a Service?

A **Service** is a program that runs **in the background** to provide functionality to the operating system or other applications.

Unlike normal programs, services usually:

- Start automatically
- Run continuously
- Do not require user interaction
- Perform specific tasks

Examples:

- SSH Server
- Web Server
- Database Server
- Network Manager

Android also has services, but they are different from Linux services and Android **Application Services**.

---

# Why are Services Important?

Services provide:

- Background functionality
- System management
- Network communication
- Hardware management
- Process management

Without services, Linux and Android could not function properly.

---

# Service vs Process

| Service | Process |
|----------|----------|
| Performs a background task | Running instance of a program |
| Usually runs continuously | Can be short-lived or long-running |
| Usually starts automatically | Starts when a program is executed |
| Every service runs as a process | Not every process is a service |

---

# Service Lifecycle

```text
System Boot
      ↓
Service Starts
      ↓
Running
      ↓
Handles Requests
      ↓
Stops
```

---

# Types of Linux Services

## System Services

Provide core operating system functionality.

Examples:

- SSH
- Cron
- Network Manager
- D-Bus
- Logging

---

## Network Services

Provide network functionality.

Examples:

- Web Server
- FTP Server
- DNS Server
- DHCP Server

---

## Application Services

Provide functionality for applications.

Examples:

- Database Server
- Redis
- Docker

---

# Service Manager

Most modern Linux distributions use **systemd** to manage services.

Responsibilities:

- Start services
- Stop services
- Restart services
- Monitor services
- Start services during boot

Example:

```text
systemd
      ↓
Starts Services
```

---

# Useful Service Commands

List services:

```bash
systemctl list-units --type=service
```

Check service status:

```bash
systemctl status ssh
```

Start a service:

```bash
systemctl start ssh
```

Stop a service:

```bash
systemctl stop ssh
```

Restart a service:

```bash
systemctl restart ssh
```

Enable at boot:

```bash
systemctl enable ssh
```

Disable:

```bash
systemctl disable ssh
```

---

# Service Example

SSH Server

```text
User
    ↓
SSH Request
    ↓
SSH Service
    ↓
Authentication
    ↓
Shell Access
```

---

# Android Services

Android uses several kinds of services.

1. Linux System Services
2. Android System Services
3. Android Application Services

These are different concepts.

---

# Linux System Services in Android

Examples:

- init
- adbd
- logd
- vold
- servicemanager

These are background processes that help Android operate.

---

# Android System Services

Android Framework provides many **System Services**.

Examples:

| Service | Purpose |
|----------|----------|
| Activity Manager | Manages activities and processes |
| Package Manager | Installs and manages apps |
| Window Manager | Manages windows |
| Notification Manager | Displays notifications |
| Power Manager | Controls power usage |
| Location Manager | GPS and location |
| Sensor Manager | Device sensors |
| Connectivity Manager | Network connectivity |

Applications communicate with these services using **Binder IPC**.

---

# Android System Service Flow

```text
Application
      ↓
Framework API
      ↓
Binder IPC
      ↓
System Service
      ↓
HAL
      ↓
Linux Kernel
      ↓
Hardware
```

---

# Android Application Service

An **Android Service** is one of the four main Android application components.

It performs long-running work in the background.

Examples:

- Music playback
- File downloads
- GPS tracking
- Data synchronization

Example:

```text
Music App
      ↓
Background Service
      ↓
Music Continues
```

---

# Difference Between Linux Services and Android Services

| Linux Service | Android Application Service |
|---------------|-----------------------------|
| Operating system component | Android app component |
| Managed by systemd or init | Managed by Android Framework |
| Runs continuously | Runs as needed by an app |
| Provides OS functionality | Provides app functionality |

---

# Android init Process

The first user-space process in Android is:

```text
init
```

Responsibilities:

- Mount file systems
- Start system services
- Read `init.rc`
- Configure the device

Flow:

```text
Bootloader
      ↓
Linux Kernel
      ↓
init
      ↓
Start Services
```

---

# Android Service Manager

Android uses **servicemanager** to register and manage Binder services.

Example:

```text
Application
      ↓
Binder
      ↓
Service Manager
      ↓
Activity Manager Service
```

---

# Useful Android Commands

List system services:

```bash
adb shell service list
```

View running processes:

```bash
adb shell ps -A
```

Dump Activity Manager:

```bash
adb shell dumpsys activity
```

Dump Package Manager:

```bash
adb shell dumpsys package
```

Dump all services:

```bash
adb shell dumpsys
```

---

# Services from a Pentester's Perspective

System services are high-value targets because they:

- Run with elevated privileges
- Communicate using Binder IPC
- Manage sensitive operations
- Handle user data

---

# Common Attack Surface

- Binder IPC
- Exported Android Services
- Privileged System Services
- Permission Checks
- Intent Handling

---

# Example: Exported Android Service

```text
Malicious App
      ↓
Bind to Exported Service
      ↓
Sensitive Operation
```

If the service lacks proper permission checks, an attacker may trigger privileged functionality.

---

# Example: Binder Attack

```text
Malicious App
      ↓
Binder Transaction
      ↓
System Service
      ↓
Privilege Escalation
```

---

# Example: Service Misconfiguration

```text
Exported Service
        ↓
No Permission Check
        ↓
Unauthorized Access
```

---

# Complete Android Flow

```text
User Opens App
        ↓
Application
        ↓
Framework API
        ↓
Binder IPC
        ↓
System Service
        ↓
HAL
        ↓
Linux Kernel
        ↓
Hardware
```

---

# Linux Services vs Android System Services

| Linux Services | Android System Services |
|----------------|--------------------------|
| Started by `init` or `systemd` | Started during Android boot |
| Background operating system processes | Framework components providing Android APIs |
| Examples: `adbd`, `logd`, `vold` | Activity Manager, Package Manager |
| May communicate using sockets or other IPC | Primarily communicate using Binder IPC |

---

# Key Takeaways

- A **Linux Service** is a background program that provides operating system functionality.
- Every service runs as a **process**, but not every process is a service.
- Android includes **Linux system services**, **Android System Services**, and **Android Application Services**, each serving different purposes.
- Android System Services expose core functionality (such as package management, activity management, and location) through **Binder IPC**.
- During Android penetration testing, services are an important attack surface because vulnerabilities in exported services, Binder interfaces, or permission checks can lead to **privilege escalation**, **information disclosure**, or **unauthorized access**.
