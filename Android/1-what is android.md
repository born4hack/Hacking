# What is Android?

## Definition

**Android** is an **open-source mobile operating system (OS)** developed by **Google**. It is designed primarily for smartphones and tablets, but it is also used in TVs, watches, cars, and other smart devices.

Think of Android as the software that sits between the hardware and the apps.

Example:

```
Instagram
WhatsApp
Chrome
YouTube
      │
      ▼
Android Operating System
      │
      ▼
Phone Hardware
(CPU, RAM, Camera, GPS, Storage)
```

Without Android, apps cannot easily communicate with the phone's hardware.

---

# Simple Example

When you open the Camera app:

```
Camera App
     │
     ▼
Android OS
     │
     ▼
Camera Hardware
```

The app doesn't talk directly to the camera.

Instead:

1. Camera app requests Android.
2. Android checks permissions.
3. Android communicates with the hardware.
4. Android returns the image.

---

# Why Android Exists

Android provides:

- User Interface
- Memory Management
- Process Management
- File System
- Security
- Networking
- App Management
- Permission System
- Hardware Access

Instead of every app controlling hardware directly, Android manages everything safely.

---

# Main Responsibilities of Android

## 1. Run Applications

Android launches and manages apps.

Example:

```
You open WhatsApp

↓

Android starts the WhatsApp process.

↓

WhatsApp appears on screen.
```

---

## 2. Manage Memory (RAM)

Android decides:

- Which apps stay open
- Which apps should be closed
- Which apps receive more memory

Example:

```
Open:

Chrome
YouTube
Instagram
WhatsApp

↓

RAM becomes full

↓

Android automatically kills the least important app.
```

---

## 3. Manage CPU

Android decides:

- Which app gets CPU time
- Background tasks
- Foreground tasks

---

## 4. Manage Storage

Android stores:

- Photos
- Videos
- APKs
- Databases
- Cache
- Logs

---

## 5. Security

Android isolates every app.

Every app runs separately.

```
Instagram

cannot directly access

WhatsApp data
```

This is called **Sandboxing**.

---

## 6. Permission Management

Apps must ask permission before accessing:

- Camera
- Microphone
- GPS
- Contacts
- SMS
- Files

Example:

```
Instagram

↓

Request Camera Permission

↓

Android asks user

↓

Allow or Deny
```

---

## 7. Networking

Android provides:

- Wi-Fi
- Bluetooth
- Mobile Data
- VPN
- DNS
- HTTPS

Apps use Android networking APIs.

---

## 8. Hardware Access

Android communicates with:

- Camera
- GPS
- NFC
- Accelerometer
- Fingerprint Sensor
- Microphone
- Speaker
- Display
- USB

---

# Android Architecture (High Level)

```
Apps
──────────────────────

Android Framework
──────────────────────

Android Runtime (ART)
Native Libraries
──────────────────────

Hardware Abstraction Layer (HAL)
──────────────────────

Linux Kernel
──────────────────────

Hardware
```

Later, you'll study each layer in detail.

---

# Android Version Flow

Example:

```
Android 12

↓

Android 13

↓

Android 14

↓

Android 15
```

Each version introduces:

- New APIs
- Security improvements
- Permission changes
- New features

---

# Android Apps

Android applications are packaged as:

```
APK
```

APK stands for:

```
Android Package
```

Example:

```
Instagram.apk
WhatsApp.apk
Bank.apk
```

An APK contains:

- Code
- Images
- Resources
- Manifest
- Native libraries
- Assets

---

# Android Components (Important)

Every Android app is built using four main components:

1. Activities
2. Services
3. Broadcast Receivers
4. Content Providers

You'll learn each in depth because they are common attack surfaces in Android penetration testing.

---

# Android Security Features

Android includes several built-in security mechanisms:

- App Sandbox
- Linux User IDs (UIDs)
- Permissions
- SELinux
- Verified Boot
- File Encryption
- Play Protect
- Scoped Storage
- Secure IPC (Binder)
- Network Security Configuration

Understanding these is essential for finding vulnerabilities.

---

# Why Learn Android for Penetration Testing?

Most Android vulnerabilities arise from insecure implementations of Android components and APIs.

Common vulnerability areas include:

- Exported Activities
- Exported Services
- Broadcast Receiver abuse
- Content Provider exposure
- Deep Link vulnerabilities
- WebView issues
- Insecure Data Storage
- Weak Cryptography
- Improper Authentication
- Insecure Network Communication
- Intent Injection
- Task Hijacking
- Hardcoded Secrets
- Root Detection Bypass
- SSL Pinning Bypass

Learning how Android works helps you identify these issues effectively.

---

# Real-World Example

Suppose a banking app stores authentication tokens in plaintext.

```
Bank App

↓

Stores token in SharedPreferences

↓

Attacker extracts APK data

↓

Reads token

↓

Uses token to access victim's account
```

This is an example of **Insecure Data Storage**.

---

# Key Terms

| Term | Meaning |
|------|---------|
| Android | Mobile operating system developed by Google |
| OS | Operating System |
| APK | Android application package |
| App Sandbox | Isolates apps from each other |
| Permission | User-granted access to sensitive resources |
| Activity | A screen in an app |
| Service | Background task |
| Broadcast Receiver | Responds to system or app events |
| Content Provider | Shares structured app data |
| Linux Kernel | Core layer managing hardware and system resources |
| HAL | Hardware Abstraction Layer |
| ART | Android Runtime |

---

# Summary

- Android is an operating system for mobile devices.
- It manages hardware, apps, security, memory, storage, and networking.
- Apps run inside isolated sandboxes.
- APK files contain Android applications.
- Android is built on the Linux kernel.
- Every Android app consists of core components such as Activities, Services, Broadcast Receivers, and Content Providers.
- A strong understanding of Android fundamentals is essential before learning Android penetration testing.

---

# Next Topics to Learn

1. Linux Basics
2. Android Architecture (Detailed)
3. Android File System
4. APK Structure
5. Android Application Components
6. Android Permissions
7. Android App Lifecycle
8. Intents
9. Binder IPC
10. Android Security Model
