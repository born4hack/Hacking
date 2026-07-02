# Android Boot Process

## What is the Android Boot Process?

The **Android Boot Process** is the sequence of steps that occur from the moment you press the **power button** until the Android home screen appears.

It initializes the hardware, loads the Linux kernel, starts essential system services, and finally launches the Android user interface.

Example:

```text
Power Button Pressed
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
init Process
        │
        ▼
Mount File Systems
        │
        ▼
Start Android Services
        │
        ▼
Zygote
        │
        ▼
System Server
        │
        ▼
Launcher (Home Screen)
```

Without the boot process, Android cannot start or run applications.

---

# Why Learn the Boot Process?

Understanding the Android boot process is important because many Android security mechanisms are initialized during boot.

For penetration testers, it helps understand:

- Verified Boot
- dm-verity
- SELinux initialization
- Rooting process
- Bootloader unlocking
- Custom ROMs
- Kernel exploitation
- Recovery mode
- Fastboot mode

Many Android attacks target different stages of the boot process.

---

# Overview of the Android Boot Process

The complete boot process consists of the following stages:

1. Power On
2. Boot ROM
3. Bootloader
4. Linux Kernel
5. init Process
6. Mount File Systems
7. Start Native Daemons
8. Zygote
9. System Server
10. Launcher (Home Screen)

---

# Step 1 — Power On

Everything begins when the user presses the power button.

Example:

```text
User

↓

Presses Power Button

↓

CPU receives power

↓

Starts executing instructions
```

At this point:

- CPU starts running
- RAM is initialized
- Internal storage becomes accessible
- Execution begins from a predefined memory location

---

# Step 2 — Boot ROM

Every Android device contains a small piece of firmware called the **Boot ROM**.

It is permanently stored inside the processor (SoC).

Its job is to find and execute the bootloader.

Example:

```text
Power On

↓

Boot ROM

↓

Search Bootloader

↓

Load Bootloader into RAM

↓

Execute Bootloader
```

The Boot ROM cannot usually be modified.

Responsibilities:

- Hardware initialization
- Verify bootloader (on secure devices)
- Load first-stage bootloader

---

# Step 3 — Bootloader

The bootloader is responsible for preparing the system before Android starts.

Example:

```text
Bootloader

↓

Initialize Hardware

↓

Verify Boot Image

↓

Load Linux Kernel

↓

Pass Control to Kernel
```

The bootloader initializes:

- CPU
- RAM
- Display
- Storage
- USB
- Buttons

---

## Locked Bootloader

Most Android devices ship with a locked bootloader.

```
Only trusted software

↓

Can boot
```

This prevents attackers from installing malicious operating systems.

---

## Unlocked Bootloader

When unlocked:

```
User Unlocks Bootloader

↓

Any boot image

↓

Can be loaded
```

This allows:

- Rooting
- Custom recovery
- Custom ROMs
- Kernel modifications

It also weakens some security guarantees.

---

# Android Verified Boot (AVB)

Before loading Android, modern devices verify that critical partitions have not been modified.

Example:

```text
Bootloader

↓

Verify Boot Image

↓

Signature Valid?

      │
 ┌────┴────┐
 │         │
Yes        No
 │         │
 ▼         ▼
Boot      Stop / Warning
```

Verified Boot helps prevent attackers from replacing the operating system with a malicious version.

---

# Step 4 — Linux Kernel

Once verification succeeds, the bootloader loads the Linux kernel into memory.

Example:

```text
Bootloader

↓

Linux Kernel Starts

↓

Initialize Drivers

↓

Memory Management

↓

Process Scheduler

↓

Start init
```

The Linux kernel is responsible for:

- Process management
- Memory management
- CPU scheduling
- Device drivers
- Security
- Networking

---

# Kernel Initializes Drivers

The kernel loads drivers for hardware devices.

Example:

```text
Linux Kernel

↓

Camera Driver

↓

Display Driver

↓

Touchscreen Driver

↓

Wi-Fi Driver

↓

Bluetooth Driver

↓

USB Driver
```

Without these drivers, Android cannot interact with hardware.

---

# Step 5 — init Process

After the kernel finishes initialization, it starts the first userspace process called **init**.

Process ID:

```text
PID = 1
```

Every other Android process ultimately descends from `init`.

Example:

```text
Kernel

↓

init (PID 1)

↓

Start System Services
```

---

# What Does init Do?

The init process performs several critical tasks:

- Reads init configuration files
- Mounts partitions
- Starts native daemons
- Sets permissions
- Applies security policies
- Starts Zygote

Example:

```text
init

↓

Read init.rc

↓

Mount Partitions

↓

Start Services
```

---

# init.rc

The `init.rc` file contains startup instructions.

Example:

```text
init.rc

↓

Start adbd

↓

Start logd

↓

Start servicemanager

↓

Start vold
```

It is similar to a startup script for Android.

---

# Step 6 — Mount File Systems

Android mounts essential partitions.

Example:

```text
/system

/vendor

/data

/cache

/product
```

These partitions contain:

| Partition | Purpose |
|-----------|---------|
| `/system` | Android operating system |
| `/vendor` | Hardware vendor files |
| `/data` | User applications and data |
| `/cache` | Temporary files |
| `/product` | Product-specific resources |

---

# Step 7 — Start Native Daemons

The init process starts important background services (daemons).

Examples include:

- adbd
- logd
- vold
- servicemanager
- healthd

Example:

```text
init

↓

Start adbd

↓

Start vold

↓

Start logd
```

Some important daemons:

### adbd

Android Debug Bridge daemon.

Allows communication between:

```text
Computer

↓

ADB

↓

Android Device
```

---

### vold

Volume Daemon.

Responsible for:

- Storage
- SD cards
- USB storage
- Mounting partitions

---

### logd

Responsible for Android logging.

Used by:

```text
logcat
```

---

### servicemanager

Registers and manages Binder services used for inter-process communication.

---

# Step 8 — Start Zygote

One of the most important steps.

The init process starts **Zygote**.

Example:

```text
init

↓

Start Zygote
```

Zygote is the parent process for almost every Android application.

---

# What is Zygote?

Instead of starting every app from scratch:

```text
New App

↓

Fork from Zygote
```

Benefits:

- Faster startup
- Lower memory usage
- Shared framework resources

---

# Zygote Flow

```text
Zygote

├── Chrome

├── WhatsApp

├── Instagram

├── Banking App

└── Settings
```

Almost every app is created by forking Zygote.

---

# Step 9 — System Server

After Zygote starts, it creates the **System Server** process.

Example:

```text
Zygote

↓

Fork

↓

System Server
```

The System Server manages most Android framework services.

---

# What Does System Server Start?

Examples include:

- Activity Manager
- Package Manager
- Window Manager
- Power Manager
- Notification Manager
- Location Manager
- Sensor Manager
- Connectivity Manager

Example:

```text
System Server

↓

Activity Manager

↓

Package Manager

↓

Window Manager
```

Without the System Server, Android cannot function normally.

---

# Step 10 — Launcher (Home Screen)

Finally, Android launches the default launcher application.

Example:

```text
System Server

↓

Launcher

↓

Home Screen Appears
```

Now the device is ready for user interaction.

---

# Complete Boot Flow

```text
Power Button
      │
      ▼
Boot ROM
      │
      ▼
Bootloader
      │
      ▼
Verified Boot
      │
      ▼
Linux Kernel
      │
      ▼
init (PID 1)
      │
      ▼
Mount Partitions
      │
      ▼
Start Native Daemons
      │
      ▼
Start Zygote
      │
      ▼
Fork System Server
      │
      ▼
Start Android Framework Services
      │
      ▼
Launch Home Screen
```

---

# Android Boot Process from a Pentester's Perspective

Each stage has potential security implications:

| Stage | Security Relevance |
|--------|--------------------|
| Bootloader | Unlocking enables custom images and rooting |
| Verified Boot | Detects tampered boot and system partitions |
| Linux Kernel | Kernel vulnerabilities may allow privilege escalation |
| init | Misconfigured startup services can expose attack surfaces |
| Native Daemons | Services like `adbd` may be abused if insecurely configured |
| Zygote | Every app is forked from Zygote, making it a core part of app execution |
| System Server | Bugs here can affect the entire Android system |
| Launcher | Generally not security-critical but marks completion of boot |

---

# Real-World Example

Suppose an attacker flashes a modified boot image.

```text
Modified boot.img

↓

Bootloader

↓

Verified Boot

↓

Signature Check

↓

Fails

↓

Device refuses to boot
```

If the bootloader is unlocked and verification is disabled or bypassed:

```text
Modified boot.img

↓

Bootloader

↓

Kernel Starts

↓

Root Access Enabled
```

This is one reason why bootloader state is important during Android security assessments.

---

# Key Terms

| Term | Meaning |
|------|---------|
| Boot ROM | Immutable firmware that starts the boot process |
| Bootloader | Initializes hardware and loads the Linux kernel |
| Verified Boot | Verifies integrity of boot-critical partitions |
| Linux Kernel | Core operating system managing hardware and resources |
| init | First userspace process (PID 1) |
| init.rc | Startup configuration script for init |
| Daemon | Background system service |
| adbd | Android Debug Bridge daemon |
| vold | Volume management daemon |
| logd | Android logging daemon |
| Zygote | Parent process of nearly all Android apps |
| System Server | Starts and manages Android framework services |
| Launcher | Home screen application shown after boot |

---

# Summary

- Android starts with the Boot ROM and Bootloader.
- The bootloader verifies and loads the Linux kernel.
- The kernel initializes hardware and starts the `init` process.
- `init` mounts file systems and starts essential native daemons.
- `init` launches Zygote.
- Zygote forks the System Server and application processes.
- The System Server starts core Android framework services.
- Finally, the Launcher displays the home screen.
- Understanding the boot process is fundamental for Android internals, rooting, custom ROM development, and mobile penetration testing.

---
