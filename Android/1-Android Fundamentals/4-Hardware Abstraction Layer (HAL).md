# Hardware Abstraction Layer (HAL) - Android Pentesting

## What is HAL?

The **Hardware Abstraction Layer (HAL)** is a software layer between the **Android Framework** and the **hardware-specific drivers**.

Its main purpose is to **hide hardware implementation details** from the Android Framework.

Without HAL, Android would need different framework code for every device manufacturer.

---

# Position in Android Architecture

```text
+---------------------------+
|       Applications        |
+---------------------------+
|   Application Framework   |
+---------------------------+
|     Native Libraries      |
|     Android Runtime       |
+---------------------------+
|  Hardware Abstraction     |
|         Layer (HAL)       |
+---------------------------+
|       Linux Kernel        |
+---------------------------+
|         Hardware          |
+---------------------------+
```

---

# Why HAL Exists

Every Android manufacturer uses different hardware.

For example:

| Device | Camera Sensor |
|---------|---------------|
| Samsung | Sony IMX989 |
| Xiaomi | OmniVision |
| Google Pixel | Samsung GN2 |

The Android Camera API should work the same on all devices.

Instead of changing Android, manufacturers only implement their own HAL.

---

# Without HAL

Android Framework would need device-specific code.

```text
Camera App
      ↓
If Samsung → Samsung Driver

If Xiaomi → Xiaomi Driver

If Pixel → Pixel Driver
```

This would be impossible to maintain.

---

# With HAL

Android always talks to HAL.

```text
Camera App
      ↓
Camera Framework
      ↓
Camera HAL
      ↓
Linux Kernel Driver
      ↓
Camera Hardware
```

Framework code never changes.

---

# Simple Example

Suppose a camera app wants to take a photo.

Flow:

```text
Camera App
      ↓
CameraManager API
      ↓
Camera Service
      ↓
Camera HAL
      ↓
Camera Driver
      ↓
Camera Sensor
```

The HAL translates generic Android requests into hardware-specific operations.

---

# Responsibilities of HAL

HAL provides a standard interface for hardware.

Examples:

- Camera
- Audio
- GPS
- Bluetooth
- Wi-Fi
- NFC
- Fingerprint
- Face Unlock
- Sensors
- USB
- Vibrator

---

# Common HAL Modules

| HAL Module | Purpose |
|------------|---------|
| Camera HAL | Camera access |
| Audio HAL | Microphone and speakers |
| Bluetooth HAL | Bluetooth communication |
| Wi-Fi HAL | Wireless networking |
| GPS HAL | Location services |
| NFC HAL | NFC communication |
| Sensors HAL | Accelerometer, Gyroscope, etc. |
| USB HAL | USB devices |
| Biometrics HAL | Fingerprint and Face Unlock |
| Power HAL | Battery optimization |
| Lights HAL | LEDs and display brightness |
| Vibrator HAL | Device vibration |

---

# Camera HAL Example

```text
Camera App
      ↓
Camera API
      ↓
Camera Service
      ↓
Camera HAL
      ↓
Kernel Driver
      ↓
Camera Hardware
```

The HAL knows how to communicate with that specific camera hardware.

---

# Audio HAL Example

```text
Music App
      ↓
Audio Framework
      ↓
Audio HAL
      ↓
Audio Driver
      ↓
Speaker
```

---

# GPS HAL Example

```text
Google Maps
      ↓
Location Manager
      ↓
GPS HAL
      ↓
GPS Driver
      ↓
GPS Chip
```

---

# Fingerprint HAL Example

```text
Lock Screen
      ↓
Biometric Service
      ↓
Fingerprint HAL
      ↓
Fingerprint Driver
      ↓
Fingerprint Sensor
```

---

# HAL Interface

Android Framework communicates with HAL through defined interfaces.

Older Android versions used:

- C-based HAL

Modern Android uses:

- HIDL (HAL Interface Definition Language)
- AIDL (Android Interface Definition Language)

These provide standardized communication between the framework and HAL implementations.

---

# HAL Files

HAL implementations are usually stored as shared libraries.

Examples:

```text
/vendor/lib/hw/
```

or

```text
/vendor/lib64/hw/
```

Example files:

```text
camera.vendor.so

audio.primary.so

gps.default.so

fingerprint.default.so

bluetooth.default.so
```

These are vendor-specific implementations.

---

# HAL Communication Flow

```text
Application
      ↓
Framework API
      ↓
System Service
      ↓
HAL
      ↓
Kernel Driver
      ↓
Hardware
```

---

# HAL from a Pentester's Perspective

HAL is part of the attack surface because it processes data from hardware and communicates with privileged system services.

Possible targets include:

- Camera HAL
- Fingerprint HAL
- Bluetooth HAL
- NFC HAL
- USB HAL
- Audio HAL

---

# Why Pentesters Care About HAL

HAL often runs with elevated privileges.

A vulnerability may lead to:

- Privilege Escalation
- Information Disclosure
- Denial of Service (DoS)
- Remote Code Execution (RCE)

---

# Example Attack Surface

## Camera HAL

Possible issues:

- Memory corruption
- Buffer overflow
- Integer overflow

Flow:

```text
Malicious App
      ↓
Malformed Camera Request
      ↓
Camera HAL
      ↓
Crash or Code Execution
```

---

## Fingerprint HAL

Possible issues:

- Authentication bypass
- Information disclosure
- Logic flaws

Example:

```text
Fake Fingerprint Data
      ↓
Fingerprint HAL
      ↓
Authentication Bypass
```

---

## Bluetooth HAL

Possible issues:

- Remote code execution
- Memory corruption
- Information leakage

Flow:

```text
Bluetooth Packet
      ↓
Bluetooth HAL
      ↓
Crash or Exploit
```

---

# Useful Commands

List vendor libraries:

```bash
ls /vendor/lib64/hw/
```

or

```bash
ls /vendor/lib/hw/
```

Find HAL libraries:

```bash
find /vendor -name "*.so"
```

Check loaded libraries:

```bash
cat /proc/<PID>/maps
```

List vendor partitions:

```bash
ls /vendor
```

---

# Real-World Pentesting Example

Suppose a camera HAL does not properly validate image metadata.

```text
Malicious Camera App
        ↓
Malformed Image Request
        ↓
Camera HAL
        ↓
Memory Corruption
        ↓
Privilege Escalation
```

---

# HAL Security

Modern Android protects HAL using:

- SELinux policies
- Process isolation
- Verified Boot
- Vendor partitions
- HIDL/AIDL interface validation

These reduce the impact of vulnerabilities but do not eliminate them.

---

# Difference Between HAL and Linux Kernel

| Hardware Abstraction Layer (HAL) | Linux Kernel |
|----------------------------------|--------------|
| Provides a standard interface to hardware | Directly controls hardware |
| Vendor-specific implementation | Core operating system component |
| Translates framework requests | Executes hardware operations |
| Lives above the Linux Kernel | Lives below HAL |
| Uses kernel drivers | Contains device drivers |

---

# Complete Flow Example

```text
Instagram App
        ↓
Android Framework
        ↓
Camera Service
        ↓
Camera HAL
        ↓
Linux Kernel
        ↓
Camera Driver
        ↓
Camera Sensor
```

---

# Key Takeaways

- HAL stands for **Hardware Abstraction Layer**.
- It sits between the Android Framework and the Linux Kernel.
- HAL provides a standard interface for hardware access, allowing Android to run on devices with different hardware.
- Each hardware component (camera, audio, GPS, Bluetooth, etc.) has its own HAL implementation.
- Modern Android uses **HIDL** and **AIDL** for HAL communication.
- HAL is an important attack surface in Android penetration testing because vulnerabilities in HAL components can lead to privilege escalation, information disclosure, or remote code execution.
```
