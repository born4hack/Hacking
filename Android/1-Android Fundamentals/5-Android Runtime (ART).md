# Android Runtime (ART) - Android Pentesting

## What is Android Runtime (ART)?

**Android Runtime (ART)** is the environment that **executes Android applications**.

Every Android app written in **Java** or **Kotlin** ultimately runs on ART.

ART is responsible for:

- Running Android apps
- Memory management
- Garbage Collection (GC)
- Compiling app code
- Optimizing app performance

---

# Position in Android Architecture

```text
+---------------------------+
|       Applications        |
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

# Why ART Exists

Apps are written in:

- Java
- Kotlin

The CPU cannot understand Java or Kotlin directly.

Example:

```java
System.out.println("Hello");
```

The CPU only understands **machine code (binary instructions).**

ART converts app code into machine code so the CPU can execute it.

---

# Evolution of Android Runtime

| Android Version | Runtime |
|-----------------|---------|
| Android 1.x - 4.4 | Dalvik Virtual Machine (DVM) |
| Android 5.0+ | Android Runtime (ART) |

ART replaced Dalvik because it offers:

- Faster app startup
- Better performance
- Lower battery consumption
- Improved memory management

---

# How an Android App Runs

Suppose an app contains:

```kotlin
println("Hello Android")
```

Execution flow:

```text
Kotlin Code
      ↓
Java Bytecode (.class)
      ↓
D8/R8 Compiler
      ↓
DEX Bytecode (.dex)
      ↓
APK
      ↓
Android Runtime (ART)
      ↓
Machine Code
      ↓
CPU
```

---

# APK Structure

Example APK:

```text
app.apk
│
├── AndroidManifest.xml
├── classes.dex
├── resources.arsc
├── res/
├── assets/
└── lib/
```

Notice:

```text
classes.dex
```

ART executes **DEX (Dalvik Executable)** files.

---

# What is DEX?

DEX stands for:

> **Dalvik Executable**

Android does **not** execute `.java` files.

It executes:

```text
classes.dex
```

Multiple DEX files are possible:

```text
classes.dex

classes2.dex

classes3.dex
```

---

# ART Execution Flow

```text
APK
   ↓
classes.dex
   ↓
Android Runtime (ART)
   ↓
Compiled Machine Code
   ↓
CPU Execution
```

---

# ART Responsibilities

ART performs several important tasks:

- Executes applications
- Loads DEX files
- Compiles code
- Memory allocation
- Garbage Collection
- Thread management
- Exception handling
- Class loading

---

# Compilation in ART

ART uses multiple compilation strategies.

## 1. AOT (Ahead-of-Time)

Compilation occurs during app installation.

```text
Install APK
      ↓
ART
      ↓
Machine Code
      ↓
Stored on Device
```

Advantages:

- Faster startup
- Better performance

---

## 2. JIT (Just-in-Time)

Frequently used code is compiled while the app is running.

```text
Run App
      ↓
Frequently Used Method
      ↓
ART Compiles It
```

Advantages:

- Better optimization
- Faster execution over time

---

## 3. Profile-Guided Compilation

ART records frequently executed methods.

Later it recompiles only important code.

Result:

- Better battery life
- Faster apps
- Smaller storage usage

---

# Garbage Collection (GC)

Apps continuously create objects.

Example:

```java
String name = "Android";
```

When objects are no longer needed:

```text
Object
   ↓
Unused
   ↓
Garbage Collector
   ↓
Memory Freed
```

GC prevents memory leaks.

---

# Memory Allocation

ART manages:

- Heap memory
- Stack memory
- Object allocation

Example:

```text
App
   ↓
Requests Memory
   ↓
ART
   ↓
RAM
```

---

# Class Loader

ART loads classes only when needed.

Example:

```text
classes.dex
      ↓
Class Loader
      ↓
Memory
```

Benefits:

- Lower RAM usage
- Faster startup

---

# Thread Management

ART manages application threads.

Example:

```text
Main Thread

Worker Thread

Network Thread

Background Thread
```

Each thread executes independently.

---

# Exception Handling

Example:

```java
try {
    int a = 10 / 0;
} catch(Exception e) {
}
```

ART handles Java exceptions during execution.

---

# ART from a Pentester's Perspective

ART is important because it executes application code.

Pentesters analyze:

- DEX files
- Runtime behavior
- Loaded classes
- Memory
- Reflection
- Dynamic code loading

---

# Why Pentesters Care About ART

Common targets include:

- Runtime hooking
- Method interception
- Dynamic analysis
- Malware analysis
- Anti-debugging
- Obfuscation bypass

---

# Runtime Hooking

Tools such as **Frida** can hook ART methods.

Example:

```text
Target App
      ↓
ART
      ↓
Hook Java Method
      ↓
Observe Parameters
```

---

# Dynamic Analysis

Instead of reading code statically:

```text
APK
    ↓
Run App
    ↓
ART Executes Code
    ↓
Observe Behavior
```

Useful for:

- Authentication
- Encryption
- API calls
- Token generation

---

# Reflection

Java Reflection allows apps to call methods dynamically.

Example:

```java
Class.forName("com.example.Test");
```

Malware often abuses reflection to hide functionality.

---

# Dynamic Code Loading

Apps may load code at runtime.

Example:

```text
Download DEX
      ↓
Load Into ART
      ↓
Execute
```

This is common in:

- Malware
- Plugin frameworks
- Packers

---

# ART Security Features

ART includes several protections:

- Memory safety (for managed code)
- Class verification
- Bytecode verification
- Sandboxed execution
- Garbage collection

---

# Useful Pentesting Commands

Locate DEX files:

```bash
find /data/app -name "*.dex"
```

View installed packages:

```bash
pm list packages
```

Dump application data:

```bash
adb shell run-as com.example.app
```

Inspect running processes:

```bash
ps -A
```

---

# Common Attack Surface

- Reflection abuse
- Dynamic code loading
- Runtime hooking
- Obfuscation
- Anti-debugging
- Anti-Frida detection
- JNI interactions

---

# ART vs Dalvik

| Dalvik | ART |
|---------|-----|
| Older runtime | Modern runtime |
| Mostly JIT compilation | AOT + JIT + Profile-Guided |
| Slower startup | Faster startup |
| Higher CPU usage | Better optimization |
| Less efficient | More efficient |

---

# ART vs JVM

| JVM | ART |
|-----|-----|
| Runs Java applications | Runs Android applications |
| Executes `.class` files | Executes `.dex` files |
| Desktop/server focused | Mobile optimized |
| Standard Java runtime | Android-specific runtime |

---

# Complete Execution Flow

```text
Kotlin / Java Source Code
           ↓
Java Bytecode (.class)
           ↓
D8/R8 Compiler
           ↓
DEX Bytecode (.dex)
           ↓
APK
           ↓
Android Runtime (ART)
           ↓
Machine Code
           ↓
CPU
```

---

# Key Takeaways

- **ART (Android Runtime)** is the execution environment for Android applications.
- It replaced the **Dalvik Virtual Machine (DVM)** starting with Android 5.0.
- ART executes **DEX (.dex)** files, not Java source code.
- It uses **AOT**, **JIT**, and **Profile-Guided Compilation** for better performance.
- ART manages memory, garbage collection, threads, class loading, and exception handling.
- In Android penetration testing, ART is important for **runtime hooking**, **dynamic analysis**, **reflection analysis**, **DEX inspection**, and **bypassing anti-analysis protections**.
