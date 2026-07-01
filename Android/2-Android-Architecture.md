# Android Architecture

Android is built in **layers**, where each layer has a specific responsibility. Understanding this architecture is essential for Android penetration testing because vulnerabilities can exist at different layers.

---

# Android Architecture Diagram

```text
+------------------------------------------------------+
|                  Applications                        |
| (WhatsApp, Chrome, Banking Apps, etc.)              |
+------------------------------------------------------+
|              Application Framework                   |
| (Activity Manager, Package Manager, etc.)           |
+------------------------------------------------------+
|         Android Runtime (ART) + Native Libraries     |
| (Java Runtime, SQLite, OpenSSL, Media, etc.)        |
+------------------------------------------------------+
|         Hardware Abstraction Layer (HAL)             |
| (Camera, Audio, Bluetooth, GPS, Sensors, etc.)      |
+------------------------------------------------------+
|                 Linux Kernel                         |
| (Drivers, Memory, Process, Security, Networking)    |
+------------------------------------------------------+
|                     Hardware                         |
| (CPU, RAM, Camera, Display, Storage, GPS, etc.)     |
+------------------------------------------------------+
```

---

# Layer 1 — Applications

This is the top layer that users interact with.

Examples:

- WhatsApp
- Instagram
- Chrome
- Banking Apps
- Camera
- Contacts
- Phone
- Gmail

Example:

```text
User taps WhatsApp

↓

WhatsApp opens

↓

Android manages everything behind the scenes
```

These apps use Android APIs provided by the Application Framework.

---

## Penetration Testing Perspective

This is the primary layer you assess.

Common targets:

- APK files
- Activities
- Services
- Broadcast Receivers
- Content Providers
- Deep Links
- WebViews
- Local Storage
- Network Communication

---

# Layer 2 — Application Framework

This layer provides APIs and system services that applications use.

Think of it as the bridge between apps and the Android operating system.

Example:

```text
Camera App

↓

Application Framework

↓

Camera Hardware
```

Applications rarely communicate directly with hardware.

---

## Major Components

### Activity Manager

Responsible for:

- Starting activities
- Stopping activities
- Managing app lifecycle
- Managing tasks

Example:

```text
Open Instagram

↓

Activity Manager starts MainActivity
```

---

### Package Manager

Responsible for:

- Installing apps
- Uninstalling apps
- Updating apps
- Reading APK information
- Managing permissions

Example:

```text
Install APK

↓

Package Manager verifies

↓

Application installed
```

---

### Window Manager

Controls:

- Windows
- Popups
- Screen layout

---

### Notification Manager

Responsible for:

- Notifications
- Status bar alerts

Example:

```text
WhatsApp message

↓

Notification Manager

↓

Notification appears
```

---

### Location Manager

Provides:

- GPS
- Network Location
- Location APIs

---

### Resource Manager

Loads:

- Images
- Strings
- Layouts
- Colors
- Fonts

---

### Telephony Manager

Provides:

- SIM information
- Mobile network information
- IMEI (restricted)
- Signal status

---

### Sensor Manager

Controls:

- Accelerometer
- Gyroscope
- Compass
- Light Sensor

---

## Penetration Testing Perspective

Common issues include:

- Exported Components
- Intent Injection
- Permission Misconfiguration
- Task Hijacking
- Insecure IPC

---

# Layer 3 — Android Runtime (ART)

ART stands for:

```text
Android Runtime
```

Its job is to execute Android applications.

Before Android 5.0, Android used **Dalvik VM**.

Modern Android uses **ART**.

---

## Responsibilities

- Executes app code
- Manages memory
- Garbage collection
- Thread management
- JIT/AOT compilation

---

### Example

```text
Calculator App

↓

ART executes Java/Kotlin code

↓

CPU performs calculations
```

---

## Why ART Matters in Pentesting

Many dynamic analysis tools interact with ART.

Examples:

- Frida
- Objection
- Xposed (legacy)
- LSPosed

These tools hook Java methods at runtime.

---

# Native Libraries

These are written mostly in **C/C++** for performance.

Common libraries include:

- libc
- SQLite
- OpenSSL/BoringSSL
- Media Framework
- SurfaceFlinger
- Skia
- WebKit/Chromium components

Applications can access them through the Java Native Interface (JNI).

---

## Example

```text
Bank App

↓

SQLite Library

↓

Database stored
```

---

## Pentesting Perspective

Common issues:

- Native Buffer Overflows
- JNI Vulnerabilities
- Unsafe Native Code
- Memory Corruption
- Insecure Native Libraries

---

# Layer 4 — Hardware Abstraction Layer (HAL)

HAL stands for:

```text
Hardware Abstraction Layer
```

It acts as an interface between Android and device hardware.

Instead of every phone manufacturer writing custom Android code, they implement HAL modules.

---

## Example

```text
Camera App

↓

Framework

↓

Camera HAL

↓

Camera Driver

↓

Camera Hardware
```

---

## HAL Modules

Examples include:

- Camera HAL
- Audio HAL
- Bluetooth HAL
- GPS HAL
- NFC HAL
- Wi-Fi HAL
- USB HAL
- Sensors HAL

---

## Why HAL Exists

Different phones use different hardware.

Example:

Samsung Camera

↓

Samsung HAL

Google Pixel Camera

↓

Pixel HAL

Both provide the same interface to Android apps.

---

# Layer 5 — Linux Kernel

Android is built on the Linux kernel.

The kernel directly manages hardware and core system resources.

---

## Responsibilities

- Process Management
- Memory Management
- Device Drivers
- File System
- Networking
- Security
- Scheduling
- Power Management

---

### Device Drivers

Drivers exist for:

- Camera
- Display
- USB
- Wi-Fi
- Bluetooth
- Audio
- GPS
- Storage

---

### Process Management

Each Android app runs as a separate Linux process.

Example:

```text
WhatsApp

Process 2451

Instagram

Process 2499

Chrome

Process 2530
```

Each process is isolated from others.

---

### Memory Management

The kernel allocates RAM to each process and frees memory when needed.

---

### Networking

Handles:

- TCP
- UDP
- IP
- DNS
- Wi-Fi
- Mobile Data

---

### Security

The Linux kernel enforces:

- User IDs (UIDs)
- Process isolation
- File permissions
- SELinux
- Capabilities

---

## Pentesting Perspective

Examples include:

- Kernel Exploits
- Privilege Escalation
- Rooting
- SELinux Bypass
- Driver Vulnerabilities

---

# Layer 6 — Hardware

The bottom layer consists of the physical components of the device.

Examples:

- CPU
- RAM
- Flash Storage
- Camera
- Display
- Battery
- GPU
- NFC Chip
- GPS
- Bluetooth Chip
- Wi-Fi Chip
- Microphone
- Speaker
- Fingerprint Sensor

---

# Complete Flow Example

Suppose a user takes a photo.

```text
User

↓

Camera App

↓

Application Framework

↓

Camera HAL

↓

Linux Kernel Driver

↓

Camera Hardware

↓

Image Captured

↓

Back through the layers

↓

Camera App displays the image
```

---

# Android Architecture from a Pentester's Perspective

| Layer | Common Attack Surface |
|--------|------------------------|
| Applications | Activities, Services, Broadcast Receivers, Content Providers, WebViews, Deep Links |
| Application Framework | Intent Injection, Exported Components, Permission Issues |
| Android Runtime (ART) | Runtime Hooking (Frida, Objection), Method Instrumentation |
| Native Libraries | Buffer Overflows, JNI Bugs, Memory Corruption |
| HAL | Vendor Implementation Flaws (less common for app pentesters) |
| Linux Kernel | Privilege Escalation, Kernel Exploits, Rooting |

---

# Key Points

- Android is built in six layers.
- Applications interact with the Application Framework, not hardware directly.
- ART executes Java and Kotlin code.
- Native Libraries provide high-performance functionality.
- HAL standardizes communication with hardware.
- The Linux Kernel manages processes, memory, drivers, networking, and security.
- Understanding each layer helps identify where different types of vulnerabilities can occur.

---

# Next Topics

1. Android Boot Process
2. Android File System
3. APK Structure
4. Android App Components
5. Android Permissions
6. Intents
7. Binder IPC
8. Android Security Model
9. App Sandbox
10. Zygote Process
