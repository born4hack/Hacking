# Bootloader

## Definition

A **Bootloader** is a small program that runs immediately after the device is powered on. Its primary job is to initialize the hardware, verify the integrity of the operating system, load the Linux kernel into memory, and transfer control to it.

Think of the bootloader as the bridge between the device's firmware and the Android operating system.

Example:

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
```

Without a bootloader, Android cannot start.

---

# Why is the Bootloader Important?

The bootloader is responsible for preparing the device before Android starts.

Its responsibilities include:

- Initializing hardware
- Setting up memory
- Initializing storage
- Verifying boot images
- Loading the Linux kernel
- Passing control to the kernel

---

# Where Does the Bootloader Fit?

The Android startup sequence looks like this:

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
Linux Kernel
      │
      ▼
init (PID 1)
      │
      ▼
Android System
```

The bootloader acts as the gateway to the operating system.

---

# What Happens Inside the Bootloader?

The bootloader performs several important tasks before Android starts.

```text
Bootloader

↓

Initialize Hardware

↓

Verify Boot Image

↓

Load Linux Kernel

↓

Load Device Tree

↓

Pass Control to Kernel
```

---

# Step 1 — Initialize Hardware

Before Android can run, the bootloader initializes essential hardware components.

Examples include:

- CPU
- RAM
- Flash Storage
- Display
- USB
- Buttons
- Battery Controller

Example:

```text
Bootloader

↓

Initialize RAM

↓

Initialize Storage

↓

Initialize Display
```

Without hardware initialization, the Linux kernel cannot operate correctly.

---

# Step 2 — Verify Boot Image

Modern Android devices use **Android Verified Boot (AVB)** to ensure that only trusted software is loaded.

The bootloader checks the digital signature of the boot image.

Example:

```text
Bootloader

↓

Read boot.img

↓

Verify Signature

      │
 ┌────┴────┐
 │         │
Valid    Invalid
 │         │
 ▼         ▼
Boot     Stop / Warning
```

If verification fails, the device may:

- Refuse to boot
- Enter recovery mode
- Display a boot warning
- Require user confirmation

---

# Step 3 — Load the Linux Kernel

After successful verification, the bootloader loads the Linux kernel into RAM.

Example:

```text
boot.img

↓

Extract Kernel

↓

Copy to RAM

↓

Execute Kernel
```

At this point, the Linux kernel takes over.

---

# Step 4 — Load the Device Tree

Different Android devices have different hardware configurations.

The bootloader provides the kernel with hardware information through the **Device Tree**.

The Device Tree describes hardware such as:

- CPU
- Memory
- Camera
- Display
- USB
- GPIO
- Sensors

Example:

```text
Bootloader

↓

Load Device Tree

↓

Kernel knows device hardware
```

---

# Step 5 — Transfer Control

Finally, the bootloader transfers execution to the Linux kernel.

Example:

```text
Bootloader

↓

Jump to Kernel Entry Point

↓

Linux Kernel Starts
```

From this point onward, the bootloader's work is complete.

---

# Types of Bootloaders

Android devices commonly use a multi-stage bootloader.

Example:

```text
Boot ROM

↓

Primary Bootloader

↓

Secondary Bootloader

↓

Linux Kernel
```

---

## Primary Bootloader (PBL)

The Primary Bootloader is loaded directly by the Boot ROM.

Responsibilities:

- Basic hardware initialization
- Load Secondary Bootloader

Example:

```text
Boot ROM

↓

Primary Bootloader

↓

Secondary Bootloader
```

---

## Secondary Bootloader (SBL)

The Secondary Bootloader performs more advanced initialization.

Responsibilities:

- Initialize more hardware
- Verify boot image
- Load Linux kernel
- Support Fastboot mode
- Support Recovery mode

Example:

```text
Secondary Bootloader

↓

Verify boot.img

↓

Load Linux Kernel
```

---

# Locked Bootloader

Most Android devices are shipped with a locked bootloader.

A locked bootloader only allows officially signed software to boot.

Example:

```text
Official boot.img

↓

Verified

↓

Boot Success
```

Modified images are rejected.

```text
Modified boot.img

↓

Verification Failed

↓

Boot Denied
```

Benefits:

- Prevents malicious operating systems
- Protects user data
- Maintains device integrity

---

# Unlocked Bootloader

An unlocked bootloader allows users to install custom software.

Example:

```text
Unlocked Bootloader

↓

Custom boot.img

↓

Linux Kernel Starts
```

This enables:

- Rooting
- Custom ROMs
- Custom kernels
- Custom recovery
- Kernel debugging

---

# Bootloader Unlocking

Manufacturers often require users to unlock the bootloader manually.

Typical process:

```text
Developer Options

↓

Enable OEM Unlocking

↓

Boot into Fastboot Mode

↓

Run Unlock Command

↓

Bootloader Unlocks
```

Unlocking usually erases all user data to protect sensitive information.

---

# Bootloader Warning Screen

Many Android devices display a warning after unlocking.

Example:

```text
Bootloader Unlocked

↓

Orange State

↓

Warning Message

↓

Continue Boot
```

This informs users that the device's security has been reduced.

---

# Fastboot Mode

The bootloader provides a special maintenance mode called **Fastboot**.

Fastboot allows communication between a computer and the bootloader.

Example:

```text
Computer

↓

USB Cable

↓

Fastboot

↓

Bootloader
```

Common Fastboot operations include:

- Unlock bootloader
- Flash partitions
- Erase partitions
- Boot temporary images
- Retrieve device information

---

# Recovery Mode

The bootloader can also boot into Recovery Mode instead of Android.

Example:

```text
Bootloader

├── Android
└── Recovery
```

Recovery mode is used for:

- Factory reset
- Install updates
- Wipe cache
- Repair the system

---

# Boot Image (boot.img)

The bootloader loads the **boot.img** partition.

A typical boot image contains:

```text
boot.img

├── Linux Kernel
├── Ramdisk
└── Boot Configuration
```

The ramdisk contains files needed during early system startup.

---

# Bootloader Security

Modern Android devices implement several security features.

## Android Verified Boot (AVB)

Ensures that boot-critical partitions have not been modified.

---

## Rollback Protection

Prevents attackers from installing older, vulnerable firmware versions.

Example:

```text
Current Version

↓

Android 15

↓

Attempt Install Android 13

↓

Rejected
```

---

## Secure Boot Chain

Each stage verifies the next stage before executing it.

Example:

```text
Boot ROM

↓

Verify Bootloader

↓

Bootloader

↓

Verify boot.img

↓

Linux Kernel
```

This creates a chain of trust.

---

# Bootloader and Rooting

Most rooting methods require an unlocked bootloader.

Example:

```text
Unlock Bootloader

↓

Flash Patched boot.img

↓

Boot Device

↓

Root Access
```

Without unlocking, flashing a modified boot image is generally blocked.

---

# Bootloader from a Pentester's Perspective

Understanding the bootloader is important because it controls what software the device will trust and execute.

Common security topics include:

- Bootloader unlocking
- Verified Boot bypass attempts
- Fastboot misuse
- Custom recovery installation
- Rooting workflows
- Boot image modification
- Kernel debugging
- Forensic acquisition

---

# Real-World Example

Suppose an attacker creates a malicious boot image.

```text
Attacker

↓

Modified boot.img

↓

Flash Device

↓

Bootloader Verification

      │
 ┌────┴────┐
 │         │
Locked   Unlocked
 │         │
 ▼         ▼
Rejected  Boots Successfully
```

A locked bootloader protects the device by refusing to boot unauthorized images.

---

# Key Terms

| Term | Meaning |
|------|---------|
| Bootloader | Program that initializes hardware and loads the Linux kernel |
| Boot ROM | Immutable firmware that starts the boot process |
| Linux Kernel | Core operating system loaded by the bootloader |
| boot.img | Boot partition containing the kernel and ramdisk |
| Device Tree | Hardware description passed to the kernel |
| Fastboot | Protocol for communicating with the bootloader |
| Recovery Mode | Maintenance environment for repairs and updates |
| Locked Bootloader | Accepts only trusted, signed software |
| Unlocked Bootloader | Allows custom images to be flashed and booted |
| Android Verified Boot (AVB) | Verifies the integrity of boot-critical partitions |
| Rollback Protection | Prevents installation of older vulnerable firmware |
| Chain of Trust | Each boot stage verifies the next before execution |

---

# Key Takeaways

- The bootloader is responsible for preparing the device before Android starts.
- It initializes hardware, verifies boot images, loads the Linux kernel, and transfers control to it.
- Modern Android devices use Android Verified Boot (AVB) to ensure only trusted software is booted.
- A locked bootloader prevents unauthorized operating systems and modified boot images from running.
- Unlocking the bootloader enables rooting, custom ROMs, and custom kernels, but reduces device security.
- Fastboot and Recovery Mode are bootloader-managed environments used for maintenance and flashing firmware.
- Understanding the bootloader is essential for Android internals, rooting, firmware analysis, and mobile penetration testing.
