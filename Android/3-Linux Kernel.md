# Linux Kernel (Android Pentesting)

## What is the Linux Kernel?

The **Linux Kernel** is the **core component** of the Android operating system. It acts as a bridge between the Android software stack and the device hardware.

Every Android app communicates with the hardware **through the Linux Kernel**. Apps cannot directly access hardware like the CPU, camera, memory, or storage.

---

# Android Architecture Position

```text
+---------------------------+
|        Applications       |
+---------------------------+
|    Application Framework  |
+---------------------------+
|     Native Libraries      |
|     Android Runtime       |
+---------------------------+
|       Linux Kernel        |
+---------------------------+
|         Hardware          |
+---------------------------+
```

---

# Why Android Uses the Linux Kernel

Android uses the Linux Kernel because it already provides:

- Process management
- Memory management
- Security
- Device drivers
- Networking
- Power management
- File system support

Google modified the Linux Kernel to better support mobile devices.

---

# Responsibilities of the Linux Kernel

## 1. Process Management

Each Android app runs as a separate Linux process.

Example:

```text
WhatsApp
PID: 2103

Chrome
PID: 2241

Instagram
PID: 2350
```

The kernel:

- Creates processes
- Terminates processes
- Schedules CPU time
- Isolates processes

### Pentesting Importance

- Every app has a unique process.
- Attackers often inspect running processes.
- Root access allows viewing all processes.

Useful Commands:

```bash
ps -A
top
pidof com.example.app
```

---

## 2. Memory Management

The kernel controls RAM usage.

It:

- Allocates memory
- Frees unused memory
- Prevents apps from reading each other's memory

Example:

```text
App A → 200 MB
App B → 150 MB
App C → 300 MB
```

### Pentesting Importance

Memory corruption vulnerabilities include:

- Buffer Overflow
- Use-After-Free
- Double Free
- Heap Overflow

Useful Commands:

```bash
cat /proc/meminfo
dumpsys meminfo
```

---

## 3. Security

Android security is built on Linux security.

The kernel enforces:

- User IDs (UIDs)
- Permissions
- Process isolation
- SELinux
- Capabilities

Example:

```text
Instagram UID = 10234

WhatsApp UID = 10345
```

Instagram cannot access WhatsApp's private files.

### Pentesting Importance

Many privilege escalation vulnerabilities target the kernel.

Example:

```text
Normal App
      ↓
Kernel Vulnerability
      ↓
Root Access
```

---

## 4. Device Drivers

Drivers allow Android to communicate with hardware.

Examples:

| Hardware | Driver |
|----------|--------|
| Camera | Camera Driver |
| Display | Display Driver |
| Touchscreen | Touch Driver |
| Bluetooth | Bluetooth Driver |
| Wi-Fi | Wi-Fi Driver |
| USB | USB Driver |
| Audio | Audio Driver |

Example Flow:

```text
Camera App
     ↓
Camera API
     ↓
Linux Kernel
     ↓
Camera Driver
     ↓
Camera Hardware
```

### Pentesting Importance

Driver bugs can lead to:

- Privilege escalation
- Information disclosure
- Kernel crashes
- Remote code execution (RCE)

---

## 5. File System Management

Android stores everything as files.

Examples:

```text
/data
/system
/vendor
/sdcard
/proc
/sys
```

The kernel manages:

- File creation
- File deletion
- Read/write permissions
- Mounting partitions

Useful Commands:

```bash
mount
ls /
cat /proc/mounts
```

### Pentesting Importance

Common checks:

- World-readable files
- World-writable files
- Misconfigured permissions

---

## 6. Networking

The kernel manages all network communication.

Examples:

- Wi-Fi
- Mobile data
- VPN
- TCP
- UDP
- IPv4
- IPv6

Useful Commands:

```bash
ip addr
ip route
netstat
ss
```

### Pentesting Importance

Network traffic analysis is important for:

- API testing
- Packet inspection
- VPN bypass analysis
- Socket monitoring

---

## 7. Power Management

The kernel controls battery usage.

Responsibilities:

- CPU frequency scaling
- Sleep mode
- Wake locks
- Battery optimization

---

## 8. Inter-Process Communication (IPC)

Android apps communicate using:

- Binder IPC
- Unix Sockets
- Pipes
- Signals

Binder is the primary IPC mechanism in Android.

Example:

```text
App
   ↓
Binder
   ↓
System Service
```

### Pentesting Importance

Binder vulnerabilities may allow:

- Privilege escalation
- Information disclosure
- Denial of Service (DoS)

---

## 9. Hardware Abstraction

The kernel provides a consistent interface for hardware access.

Example:

```text
Application
      ↓
Camera API
      ↓
Kernel
      ↓
Samsung Camera Driver

or

Application
      ↓
Camera API
      ↓
Kernel
      ↓
Google Pixel Camera Driver
```

Apps do not need to know which hardware vendor is used.

---

# Important Linux Kernel Components

| Component | Purpose |
|-----------|---------|
| Scheduler | Decides which process uses the CPU |
| Memory Manager | Manages RAM |
| Virtual Memory | Maps memory safely |
| Device Drivers | Communicate with hardware |
| VFS (Virtual File System) | Handles file systems |
| Network Stack | Handles network traffic |
| Security Module | SELinux and permissions |
| Binder Driver | Android IPC |

---

# Linux Kernel Security Features

## User Isolation

Each app has a unique Linux UID.

```text
App A
UID 10001

App B
UID 10045
```

Apps cannot directly access each other's data.

---

## SELinux

Security-Enhanced Linux restricts actions even for privileged processes.

Modes:

- Enforcing
- Permissive
- Disabled

Check mode:

```bash
getenforce
```

---

## Permissions

The kernel enforces file permissions.

Example:

```text
-rw-------
```

Meaning:

- Owner: Read + Write
- Group: No access
- Others: No access

---

## Namespaces

Separate resources such as:

- Processes
- Mount points
- Network
- IPC

---

# Linux Kernel in Android Boot Process

```text
Power Button
      ↓
Boot ROM
      ↓
Bootloader
      ↓
Linux Kernel
      ↓
init
      ↓
System Services
      ↓
Android Framework
      ↓
Launcher
```

---

# Linux Kernel from a Pentester's Perspective

A pentester examines:

- Kernel version
- Kernel vulnerabilities
- SELinux status
- Running processes
- Loaded kernel modules
- Device drivers
- File permissions
- Binder communication
- Network interfaces
- Root status

Useful Commands:

```bash
uname -a
```

```bash
cat /proc/version
```

```bash
getenforce
```

```bash
ps -A
```

```bash
mount
```

```bash
lsmod
```

```bash
ip addr
```

---

# Common Linux Kernel Attack Surface

- Device Drivers
- Binder Driver
- File System
- Network Stack
- Bluetooth Stack
- USB Drivers
- Wi-Fi Drivers
- Camera Drivers
- GPU Drivers
- Kernel Modules

---

# Real Pentesting Examples

### Example 1: Privilege Escalation

```text
App
    ↓
Kernel Vulnerability
    ↓
Root Access
```

---

### Example 2: Driver Exploitation

```text
App
   ↓
Camera Driver Bug
   ↓
Kernel Memory Corruption
   ↓
Privilege Escalation
```

---

### Example 3: Binder Vulnerability

```text
Malicious App
      ↓
Malformed Binder Transaction
      ↓
System Service Crash
```

---

# Key Takeaways

- The Linux Kernel is the core of Android.
- It manages hardware, memory, processes, networking, security, and device drivers.
- Every Android app interacts with hardware through the kernel.
- Android's app sandbox is enforced by the Linux Kernel using UIDs and SELinux.
- Kernel vulnerabilities are high impact because they can lead to privilege escalation, root access, or full device compromise.
- Understanding the Linux Kernel is essential for Android penetration testing, exploit development, and vulnerability research.
