
# Android Recovery

## Definition

**Android Recovery** is a separate, lightweight bootable environment that allows you to perform maintenance, repair, and recovery operations on an Android device without booting the main Android operating system.

Think of Recovery as Android's "maintenance mode."

Example:

```text
Normal Boot

Power On
    │
    ▼
Bootloader
    │
    ▼
Android System


Recovery Boot

Power On
    │
    ▼
Bootloader
    │
    ▼
Recovery
```

Even if Android is damaged and cannot boot, Recovery can usually still start.

---

# Why Does Android Have Recovery?

Recovery provides a safe environment for repairing or maintaining the device.

Common uses include:

- Factory Reset
- Install OTA Updates
- Wipe Cache
- Repair System
- Install Custom ROMs
- Flash ZIP Packages
- Backup and Restore (Custom Recovery)

Without Recovery, many software issues would require reflashing the entire device.

---

# Where Does Recovery Fit?

Recovery is a separate boot option managed by the bootloader.

```text
Power On
    │
    ▼
Bootloader
   ├──────────────┐
   │              │
   ▼              ▼
Android      Recovery
```

The bootloader decides whether to boot Android or Recovery.

---

# What is the Recovery Partition?

Android devices have a dedicated partition called the **Recovery Partition**.

Example:

```text
Storage

├── boot
├── system
├── vendor
├── recovery
├── data
└── cache
```

The recovery partition contains:

- Recovery Kernel
- Recovery Ramdisk
- Recovery Executable
- Maintenance Tools

Unlike the normal Android system, Recovery is small and only includes essential components.

> **Note:** On many modern Android devices using the A/B (Seamless Updates) partition scheme, there may not be a dedicated `recovery` partition. Instead, Recovery is included within the `boot` or `vendor_boot` image.

---

# How Recovery Boots

When Recovery is selected:

```text
Power Button

↓

Boot ROM

↓

Bootloader

↓

Recovery Image

↓

Recovery Kernel

↓

Recovery Ramdisk

↓

Recovery Menu
```

Notice that Android itself is not started.

---

# Recovery Environment

Recovery runs independently from Android.

Example:

```text
Recovery

↓

Minimal Linux Kernel

↓

Basic Drivers

↓

Recovery Tools
```

It has:

- Small user interface
- Limited commands
- No installed apps
- No Android launcher
- No user services

---

# Stock Recovery

Every Android device comes with a **Stock Recovery** provided by the manufacturer.

Example:

```text
Samsung Recovery

Google Recovery

OnePlus Recovery

Xiaomi Recovery
```

Stock Recovery provides only basic maintenance features.

Typical options include:

- Reboot System
- Factory Reset
- Apply OTA Update
- Wipe Cache
- Power Off

---

# Custom Recovery

A **Custom Recovery** replaces the stock recovery with a more powerful version.

Popular custom recoveries include:

- TWRP (Team Win Recovery Project)
- OrangeFox Recovery
- PitchBlack Recovery

Example:

```text
Bootloader

↓

Custom Recovery

↓

Advanced Recovery Features
```

Custom recoveries provide much greater flexibility than stock recovery.

---

# Common Recovery Functions

## 1. Reboot System

Starts Android normally.

Example:

```text
Recovery

↓

Reboot System

↓

Android Starts
```

---

## 2. Factory Reset

Deletes user data and restores factory settings.

Example:

```text
Recovery

↓

Factory Reset

↓

Erase /data

↓

Restart Android
```

Typically erased:

- Installed Apps
- User Settings
- Messages
- Photos (depending on device and storage location)
- Accounts

Usually preserved:

- Android OS
- Bootloader
- Recovery

---

## 3. Wipe Cache

Deletes temporary cached files.

Example:

```text
Recovery

↓

Wipe Cache

↓

Delete Temporary Files
```

This can help resolve certain software issues.

---

## 4. Apply OTA Update

Recovery installs official software updates.

Example:

```text
OTA Package

↓

Recovery

↓

Verify Signature

↓

Install Update
```

Only properly signed updates are accepted by stock recovery.

---

## 5. Sideload Update

Recovery supports installing updates from a computer using ADB.

Example:

```text
Computer

↓

ADB Sideload

↓

Recovery

↓

Install Update
```

This is useful when Android cannot boot.

---

# Custom Recovery Features

Custom recoveries offer many advanced capabilities.

Examples:

- Flash ZIP Files
- Install Custom ROMs
- Flash Magisk
- Install Custom Kernels
- Backup Partitions
- Restore Partitions
- Mount Partitions
- File Manager
- Terminal
- ADB Access

Example:

```text
Custom Recovery

├── Install ZIP
├── Backup
├── Restore
├── Mount
├── File Manager
└── Terminal
```

---

# Nandroid Backup

One of the most valuable features of a custom recovery is creating a **Nandroid Backup**.

A Nandroid backup is a complete snapshot of the device.

Example:

```text
Backup

├── boot
├── system
├── vendor
├── data
└── recovery
```

If something goes wrong:

```text
Restore Backup

↓

Device returns

↓

Previous Working State
```

---

# Flashing ZIP Packages

Custom recoveries allow installation of ZIP packages.

Example:

```text
Recovery

↓

Install ZIP

↓

Flash Package

↓

System Modified
```

Examples of ZIP packages:

- Custom ROM
- Magisk
- Kernel
- Themes
- Security Patches

---

# Mounting Partitions

Recovery can mount Android partitions.

Example:

```text
Mount

↓

system

↓

data

↓

vendor
```

Mounted partitions can be modified or inspected.

---

# Recovery and Rooting

Recovery is commonly used during rooting.

Example:

```text
Unlock Bootloader

↓

Install Custom Recovery

↓

Flash Magisk

↓

Root Device
```

Many rooting workflows rely on custom recovery.

---

# Recovery vs Fastboot

| Feature | Recovery | Fastboot |
|----------|----------|-----------|
| Runs on Device | Yes | No |
| Requires Computer | Usually No | Usually Yes |
| Booted by Bootloader | Yes | Yes |
| Factory Reset | Yes | No |
| Flash Partitions | Custom Recovery: Yes | Yes |
| Install OTA | Yes | Limited |
| Backup System | Custom Recovery: Yes | No |

---

# Recovery from a Pentester's Perspective

Recovery is important because it provides direct access to the device outside the normal Android operating system.

Common security topics include:

- Flashing modified system images
- Installing custom ROMs
- Installing Magisk
- Creating forensic backups
- Inspecting partitions
- Data extraction
- Firmware analysis
- Recovery image modification

---

# Real-World Example

Suppose an Android device is stuck in a boot loop after a failed update.

```text
Power On

↓

Android Fails

↓

Bootloader

↓

Recovery

↓

Factory Reset

↓

Android Boots Successfully
```

If a custom recovery is installed:

```text
Recovery

↓

Restore Backup

↓

Previous System Restored

↓

Device Works Again
```

---

# Key Terms

| Term | Meaning |
|------|---------|
| Recovery | A maintenance environment separate from Android |
| Recovery Partition | Partition containing the recovery image (on devices that use one) |
| Stock Recovery | Manufacturer-provided recovery environment |
| Custom Recovery | Advanced recovery with additional features |
| OTA Update | Official over-the-air software update |
| ADB Sideload | Install update packages from a computer using ADB |
| Nandroid Backup | Complete backup of Android partitions |
| Factory Reset | Erases user data and restores factory settings |
| Flashing | Writing firmware or software to device partitions |
| Mount | Make a partition accessible for reading or writing |

---

# Key Takeaways

- Recovery is a separate bootable environment used for repairing and maintaining Android devices.
- The bootloader can boot either Android or Recovery.
- Stock Recovery supports basic operations such as factory reset and OTA updates.
- Custom Recovery adds advanced features like flashing ZIP files, backups, restores, and partition management.
- Recovery operates independently of the main Android operating system.
- Recovery plays a major role in rooting, custom ROM installation, firmware analysis, and mobile penetration testing.
- Modern A/B Android devices may not have a dedicated recovery partition because the recovery environment is integrated into the boot process.

