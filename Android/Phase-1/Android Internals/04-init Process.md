
# init Process

## Definition

The **init process** is the **first userspace process** started by the Linux kernel during the Android boot process.

It is responsible for preparing the Android operating system by mounting file systems, starting essential services, setting permissions, and launching core Android processes such as **Zygote**.

The init process always has:

```text
PID = 1
```

Every other userspace process on Android is either directly or indirectly created after `init`.

Example:

```text
Bootloader
      │
      ▼
Linux Kernel
      │
      ▼
init (PID 1)
      │
      ▼
Android Services
```

Without the init process, Android cannot start.

---

# Why is the init Process Important?

The init process acts as the "manager" of Android startup.

Its responsibilities include:

- Mounting file systems
- Starting system services
- Setting permissions
- Configuring security policies
- Launching native daemons
- Starting Zygote
- Restarting crashed services

---

# Where Does init Fit?

The Android boot sequence looks like this:

```text
Power On
      │
      ▼
Boot ROM
      │
      ▼
Bootloader
      │
      ▼
Linux Kernel
      │
      ▼
init (PID 1)
      │
      ▼
Native Daemons
      │
      ▼
Zygote
      │
      ▼
System Server
      │
      ▼
Android Apps
```

The kernel hands control to `init` after it finishes initializing hardware.

---

# What is Userspace?

Android consists of two major areas:

```text
Android System

Kernel Space

↓

Userspace
```

**Kernel Space**

- Device drivers
- Memory management
- CPU scheduling
- Security enforcement

**Userspace**

- Android services
- Applications
- Shell
- Native daemons
- Framework

The init process is the **first process in userspace**.

---

# How init Starts

After the Linux kernel finishes initialization:

```text
Linux Kernel

↓

Locate init

↓

Execute init

↓

PID = 1
```

Once started, init remains running until the device shuts down.

---

# Responsibilities of init

The init process performs several critical startup tasks.

```text
init

├── Read Configuration Files
├── Mount File Systems
├── Set Permissions
├── Configure SELinux
├── Start Native Daemons
├── Set System Properties
├── Monitor Services
└── Start Zygote
```

---

# Step 1 — Read init Configuration Files

The init process reads configuration files that define what should happen during boot.

Examples include:

```text
init.rc

init.zygote64.rc

init.usb.rc

init.vendor.rc
```

These files contain startup instructions for Android.

Example:

```text
init

↓

Read init.rc

↓

Execute Commands
```

---

# What is init.rc?

`init.rc` is a startup configuration script used by the init process.

It defines:

- Services to start
- File permissions
- Directory creation
- Mount operations
- Boot actions
- System properties

Example:

```text
init.rc

↓

Start logd

↓

Start adbd

↓

Start servicemanager

↓

Start vold
```

It works similarly to an operating system startup script.

---

# Step 2 — Mount File Systems

Before Android can run, important partitions must be mounted.

Example:

```text
init

↓

Mount /system

↓

Mount /vendor

↓

Mount /data

↓

Mount /product
```

These partitions contain:

| Partition | Purpose |
|-----------|---------|
| `/system` | Android operating system |
| `/vendor` | Vendor-specific hardware files |
| `/data` | User data and installed apps |
| `/product` | Product-specific resources |
| `/metadata` | Boot and encryption metadata |

---

# Step 3 — Create Directories

Some directories are created during boot if they do not already exist.

Example:

```text
init

↓

Create

/data/local

↓

Create

/data/misc

↓

Create

/cache
```

Permissions are also applied to these directories.

---

# Step 4 — Set Permissions

Android assigns ownership and permissions to files and directories.

Example:

```text
Directory

/data/system

↓

Owner

system

↓

Permissions

770
```

This prevents unauthorized access.

---

# Step 5 — Configure SELinux

The init process initializes **SELinux** security policies.

Example:

```text
init

↓

Load SELinux Policy

↓

Apply Security Rules
```

SELinux controls what each process is allowed to do.

For example:

```text
Camera Process

↓

Allowed

↓

Access Camera

Browser Process

↓

Denied

↓

Access Camera Device Directly
```

---

# Step 6 — Set System Properties

Android uses **System Properties** to store configuration values.

Example:

```text
ro.build.version.release

↓

15

ro.product.model

↓

Pixel 9
```

The init process loads these properties during boot.

Applications and system services can read these values.

---

# Step 7 — Start Native Daemons

One of init's biggest responsibilities is starting native background services.

Example:

```text
init

↓

Start logd

↓

Start vold

↓

Start adbd

↓

Start servicemanager
```

These services run before Android applications start.

---

# Important Native Daemons

## logd

Responsible for Android logging.

Example:

```text
Apps

↓

Generate Logs

↓

logd

↓

logcat
```

---

## adbd

Android Debug Bridge daemon.

Allows communication between a computer and the Android device.

Example:

```text
Computer

↓

ADB

↓

adbd

↓

Android Device
```

---

## vold

Volume Daemon.

Responsible for:

- Internal storage
- External storage
- SD cards
- USB storage
- Mounting partitions

---

## servicemanager

Manages **Binder IPC** services.

Example:

```text
Apps

↓

Binder

↓

Service Manager

↓

System Services
```

---

# Step 8 — Start Zygote

After preparing the system, init starts the **Zygote** process.

Example:

```text
init

↓

Start Zygote
```

Zygote is the parent process of nearly every Android application.

---

# Service Monitoring

The init process continuously monitors important services.

If a critical service crashes:

```text
Service

↓

Crash

↓

init Detects Failure

↓

Restart Service
```

This improves Android's reliability.

---

# init Service Lifecycle

Example:

```text
Start Service

↓

Running

↓

Crash

↓

Restart

↓

Running Again
```

The init process can automatically restart services based on their configuration.

---

# Android Boot Flow Around init

```text
Power On

↓

Bootloader

↓

Linux Kernel

↓

init

↓

Mount File Systems

↓

Load SELinux

↓

Start Daemons

↓

Start Zygote

↓

System Server

↓

Launcher
```

---

# init from a Pentester's Perspective

The init process is important because it controls how Android starts and which services run.

Security topics include:

- Insecure init configurations
- Startup service abuse
- SELinux policy loading
- Property manipulation
- Debug daemon exposure
- Root persistence mechanisms
- Service privilege analysis

Understanding init helps explain why certain services are available immediately after boot.

---

# Real-World Example

Suppose the Android Debug Bridge daemon (`adbd`) is configured insecurely.

```text
Device Boots

↓

init Reads init.rc

↓

Starts adbd

↓

adbd Runs with Debug Access

↓

Attacker Connects via ADB

↓

Unauthorized Device Access
```

Another example is a critical service crash.

```text
System Service

↓

Unexpected Crash

↓

init Detects Failure

↓

Automatically Restarts Service

↓

Android Continues Running
```

---

# Key Terms

| Term | Meaning |
|------|---------|
| init | First userspace process started by the Linux kernel |
| PID 1 | Process ID assigned to the init process |
| init.rc | Main startup configuration file for init |
| Userspace | Area where Android services and applications run |
| Daemon | Background service running without user interaction |
| SELinux | Mandatory access control system used by Android |
| System Properties | Configuration values used by Android |
| Zygote | Parent process of almost all Android applications |
| logd | Android logging daemon |
| adbd | Android Debug Bridge daemon |
| vold | Volume management daemon |
| servicemanager | Registers and manages Binder services |

---

# Key Takeaways

- The init process is the first userspace process started by the Linux kernel and always has **PID 1**.
- It prepares Android by mounting partitions, configuring security, setting permissions, and loading system properties.
- The init process reads configuration files such as `init.rc` to determine which actions and services should run during boot.
- It starts essential native daemons like `adbd`, `logd`, `vold`, and `servicemanager`.
- It launches the Zygote process, which later creates nearly all Android application processes.
- The init process monitors critical services and can automatically restart them if they crash.
- Understanding init is fundamental for Android internals, rooting, system debugging, and Android penetration testing.
