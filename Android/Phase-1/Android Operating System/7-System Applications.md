# System Applications - Android Pentesting

## What are System Applications?

**System Applications** are apps that come **pre-installed** with the Android operating system.

They provide the core functionality of the device and are installed by:

- Google (AOSP)
- Device manufacturers (Samsung, Xiaomi, OnePlus, etc.)
- Mobile carriers (sometimes)

Unlike normal apps, many system apps have **special privileges** and can access protected system APIs.

---

# Position in Android Architecture

```text
+---------------------------+
|    System Applications    |
|      User Applications    |
+---------------------------+
|   Application Framework   |
+---------------------------+
|     Native Libraries      |
|   Android Runtime (ART)   |
+---------------------------+
| Hardware Abstraction Layer|
+---------------------------+
|      Linux Kernel         |
+---------------------------+
|        Hardware           |
+---------------------------+
```

---

# Why System Applications Exist

Android needs built-in apps to perform essential functions.

Examples include:

- Phone calls
- SMS messaging
- Contacts
- Camera
- Clock
- Settings
- Package installation
- File management

Without these apps, a phone would not be usable.

---

# Examples of System Applications

| Application | Purpose |
|-------------|---------|
| Phone | Make and receive calls |
| Contacts | Store contacts |
| Messages | SMS/MMS |
| Camera | Take photos and videos |
| Settings | Configure the device |
| Clock | Alarms and timers |
| Calculator | Calculations |
| Calendar | Events and reminders |
| Downloads | Manage downloaded files |
| Files | File manager |
| System UI | Status bar, notifications, quick settings |
| Package Installer | Install APKs |

---

# Difference Between System Apps and User Apps

| System App | User App |
|------------|----------|
| Pre-installed | Installed by the user |
| May have privileged permissions | Uses normal app permissions |
| Can access hidden APIs | Restricted to public APIs |
| Located in system partitions | Located in `/data/app/` |
| Difficult to remove without root | Easy to uninstall |

---

# Where System Applications are Stored

Common locations:

```text
/system/app/
```

Privileged apps:

```text
/system/priv-app/
```

Newer Android versions:

```text
/product/app/
```

```text
/system_ext/app/
```

Vendor apps:

```text
/vendor/app/
```

---

# Directory Example

```text
/system
│
├── app
│   ├── Calculator
│   ├── Calendar
│   └── Contacts
│
├── priv-app
│   ├── Settings
│   ├── SystemUI
│   └── PackageInstaller
```

---

# APK Format

System applications are also APK files.

Example:

```text
Settings.apk

Camera.apk

SystemUI.apk
```

---

# How System Apps Work

Example: Opening the Camera.

```text
Camera App
      ↓
Camera API
      ↓
Camera Service
      ↓
Camera HAL
      ↓
Linux Kernel
      ↓
Camera Hardware
```

---

# Privileged Applications

Some system apps are **privileged apps**.

They can use permissions unavailable to normal apps.

Examples:

- `WRITE_SECURE_SETTINGS`
- `INSTALL_PACKAGES`
- `DELETE_PACKAGES`
- `MANAGE_USERS`
- `MODIFY_PHONE_STATE`

These permissions are usually granted only to apps in:

```text
/system/priv-app/
```

---

# SystemUI

**SystemUI** is a special system application responsible for:

- Status bar
- Notification panel
- Lock screen
- Navigation bar
- Quick Settings
- Volume controls

Flow:

```text
User
    ↓
Notification
    ↓
SystemUI
    ↓
Display
```

---

# Settings Application

The Settings app communicates with many system services.

Example:

```text
Settings
     ↓
Wi-Fi Service
     ↓
Wi-Fi HAL
     ↓
Wi-Fi Hardware
```

---

# Package Installer

The Package Installer installs APK files.

Flow:

```text
APK
   ↓
Package Installer
   ↓
Package Manager
   ↓
Application Installed
```

---

# Permissions

System apps may receive permissions automatically.

Example:

```xml
android.permission.WRITE_SECURE_SETTINGS
```

Normal applications cannot obtain this permission.

---

# System Applications from a Pentester's Perspective

System apps are a high-value target because they often:

- Run with elevated privileges
- Handle sensitive user data
- Communicate with system services
- Expose exported components

---

# Why Pentesters Care About System Apps

Compromising a system app may allow:

- Privilege Escalation
- Data Theft
- Authentication Bypass
- Persistent Malware
- Access to Protected APIs

---

# Common Attack Surface

- Exported Activities
- Exported Services
- Broadcast Receivers
- Content Providers
- Deep Links
- Intents
- IPC (Binder)
- File Handling
- Insecure Permissions

---

# Example: Exported Activity

```text
Attacker App
       ↓
Launch Exported Activity
       ↓
System App
       ↓
Sensitive Action
```

If the activity lacks proper permission checks, it may expose privileged functionality.

---

# Example: Exported Service

```text
Malicious App
       ↓
Bind to Service
       ↓
System Service
       ↓
Execute Privileged Operation
```

---

# Example: Content Provider

```text
Malicious App
      ↓
Query Provider
      ↓
System Database
      ↓
Sensitive Information
```

Improperly protected providers can leak contacts, messages, or settings.

---

# Example: Intent Injection

```text
Attacker
      ↓
Crafted Intent
      ↓
System Application
      ↓
Sensitive Function Executed
```

---

# Example: Privilege Escalation

```text
Normal App
      ↓
Exploit System App
      ↓
Gain Elevated Privileges
```

---

# Useful Commands

List installed packages:

```bash
pm list packages
```

List only system packages:

```bash
pm list packages -s
```

List third-party packages:

```bash
pm list packages -3
```

Find APK path:

```bash
pm path com.android.settings
```

Dump package information:

```bash
dumpsys package com.android.settings
```

List installed APKs:

```bash
find /system -name "*.apk"
```

---

# Reverse Engineering System Apps

Useful tools:

| Tool | Purpose |
|------|---------|
| `adb` | Interact with the device |
| `jadx` | Decompile APKs |
| `apktool` | Decode resources and manifest |
| `aapt` | APK information |
| `Frida` | Runtime instrumentation |
| `Drozer` | Component enumeration |
| `MobSF` | Static and dynamic analysis |

---

# Security Mechanisms

Android protects system apps using:

- SELinux
- App sandboxing
- Signature-based permissions
- Verified Boot
- Read-only system partitions
- Privileged permission allowlists

---

# System Apps vs User Apps

| System Apps | User Apps |
|-------------|-----------|
| Pre-installed | Installed later |
| May run with elevated privileges | Limited permissions |
| Located in system partitions | Located in `/data/app/` |
| Can use privileged permissions | Cannot use privileged permissions |
| Essential for OS functionality | Optional applications |

---

# Complete Flow Example

```text
User Opens Settings
          ↓
Settings Application
          ↓
Application Framework
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

# Key Takeaways

- **System Applications** are pre-installed Android apps that provide core device functionality.
- They are typically stored in `/system/app/`, `/system/priv-app/`, `/product/app/`, or `/system_ext/app/`.
- Some system apps are **privileged applications** and can use permissions unavailable to normal apps.
- System apps interact with the **Application Framework**, **HAL**, and the **Linux Kernel** to access hardware and system resources.
- From an Android penetration testing perspective, system applications are a critical attack surface because vulnerabilities in exported components, permissions, IPC mechanisms, or intent handling can lead to **privilege escalation**, **information disclosure**, or **unauthorized access** to sensitive functionality.
