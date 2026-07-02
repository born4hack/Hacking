# Native Libraries - Android Pentesting

## What are Native Libraries?

**Native Libraries** are precompiled libraries written mainly in **C** and **C++** that provide high-performance functionality to Android applications and the Android Framework.

Instead of writing everything in Java or Kotlin, Android uses native libraries for tasks that require:

- High performance
- Direct hardware interaction
- Complex algorithms
- Graphics processing
- Multimedia
- Database management
- Security and cryptography

These libraries are compiled into **shared object (.so)** files.

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

# Why Native Libraries Exist

Java and Kotlin are easy to develop with but are slower than native C/C++ for some tasks.

For example:

- Playing videos
- Rendering graphics
- Encrypting data
- Processing images
- Machine learning
- Database operations

Instead of implementing these in Java, Android calls optimized native libraries.

---

# Simple Example

Suppose you play a video.

Execution flow:

```text
Video App
      ↓
Media Framework
      ↓
Native Media Library
      ↓
Linux Kernel
      ↓
Hardware Decoder
```

---

# Native Library File Format

Android native libraries are stored as:

```text
lib*.so
```

Examples:

```text
libc.so

libm.so

libsqlite.so

libssl.so

libcrypto.so

libmedia.so

libandroid.so
```

`.so` means:

> **Shared Object**

It is similar to a `.dll` file on Windows.

---

# Where Native Libraries are Stored

System libraries:

```text
/system/lib/
```

64-bit libraries:

```text
/ system/lib64/
```

Vendor libraries:

```text
/vendor/lib/
```

```text
/vendor/lib64/
```

App libraries:

```text
/data/app/<package>/lib/
```

or inside the APK:

```text
lib/
├── armeabi-v7a/
├── arm64-v8a/
├── x86/
└── x86_64/
```

---

# Common Native Libraries

| Library | Purpose |
|---------|---------|
| libc.so | Standard C library |
| libm.so | Math functions |
| liblog.so | Android logging |
| libsqlite.so | SQLite database |
| libssl.so | SSL/TLS |
| libcrypto.so | Cryptography |
| libmedia.so | Audio and video |
| libz.so | Compression |
| libEGL.so | OpenGL interface |
| libGLESv2.so | OpenGL ES graphics |
| libcamera_client.so | Camera support |
| libandroid.so | Android native APIs |

---

# SQLite Library

Apps use SQLite through:

```text
Application
      ↓
SQLite API
      ↓
libsqlite.so
      ↓
SQLite Database
```

---

# OpenGL Library

Games use graphics libraries.

```text
Game
    ↓
OpenGL API
    ↓
libGLESv2.so
    ↓
GPU Driver
```

---

# SSL Library

HTTPS communication:

```text
Application
      ↓
libssl.so
      ↓
TLS Encryption
      ↓
Internet
```

---

# Media Library

Playing music:

```text
Music App
      ↓
libmedia.so
      ↓
Audio Driver
      ↓
Speaker
```

---

# Logging Library

Applications log messages through:

```text
Application
      ↓
liblog.so
      ↓
Logcat
```

---

# Native Libraries and JNI

Java cannot directly call C/C++ code.

Android uses **JNI (Java Native Interface)**.

Execution flow:

```text
Java Code
      ↓
JNI
      ↓
Native Library (.so)
      ↓
Linux Kernel
```

---

# JNI Example

Java:

```java
public native String getSecret();
```

Native C:

```c
JNIEXPORT jstring JNICALL
Java_com_example_MainActivity_getSecret(...)
```

The Java method calls the native implementation.

---

# Why Developers Use Native Code

Advantages:

- Faster execution
- Better memory control
- Hardware access
- Code reuse
- Existing C/C++ libraries
- Better game performance

---

# Native Libraries from a Pentester's Perspective

Native libraries are an important attack surface because they are written in **C/C++**, which can have memory safety vulnerabilities.

Common targets include:

- Buffer overflows
- Integer overflows
- Heap corruption
- Format string bugs
- Use-after-free
- Double free

---

# Why Pentesters Care About Native Libraries

Java code is memory-managed by ART.

Native code is **not**.

Native bugs can lead to:

- Remote Code Execution (RCE)
- Privilege Escalation
- Information Disclosure
- Application crashes

---

# Example Attack Flow

```text
Malicious Input
       ↓
Native Library (.so)
       ↓
Buffer Overflow
       ↓
Code Execution
```

---

# Native Library Reverse Engineering

Common tools:

| Tool | Purpose |
|------|---------|
| `jadx` | Analyze APK |
| `apktool` | Decode APK |
| `Ghidra` | Reverse engineer `.so` files |
| `IDA Free` | Disassemble native binaries |
| `Binary Ninja` | Native analysis |
| `objdump` | View assembly |
| `readelf` | ELF information |
| `nm` | List exported symbols |
| `strings` | Extract readable strings |

---

# Useful Commands

Extract native libraries from an APK:

```bash
unzip app.apk
```

List libraries:

```bash
find . -name "*.so"
```

View architecture:

```bash
file libexample.so
```

List exported symbols:

```bash
nm -D libexample.so
```

Read ELF information:

```bash
readelf -a libexample.so
```

Extract strings:

```bash
strings libexample.so
```

---

# Common Vulnerabilities

- Buffer Overflow
- Heap Overflow
- Stack Overflow
- Integer Overflow
- Format String Vulnerability
- Double Free
- Use-After-Free
- Memory Leak
- Race Condition

---

# Example: Buffer Overflow

```text
User Input
      ↓
Native Function
      ↓
Fixed-size Buffer
      ↓
Overflow
      ↓
Crash or Code Execution
```

---

# Example: JNI Security Issue

```text
Java App
      ↓
JNI
      ↓
Unsafe Native Code
      ↓
Memory Corruption
```

---

# Security Mechanisms

Modern Android uses several protections:

- ASLR (Address Space Layout Randomization)
- NX (Non-Executable Memory)
- PIE (Position Independent Executable)
- RELRO
- Stack Canaries
- Control Flow Integrity (CFI)
- Fortify Source

These make exploitation harder but not impossible.

---

# Native Libraries vs ART

| Native Libraries | Android Runtime (ART) |
|------------------|-----------------------|
| Written in C/C++ | Executes Java/Kotlin code |
| Compiled to `.so` files | Executes `.dex` files |
| Accessed through JNI | Manages app execution |
| Faster but less memory-safe | Memory-managed environment |
| Vulnerable to memory corruption | Protected by garbage collection |

---

# Complete Execution Flow

```text
Java/Kotlin Application
          ↓
Java Method
          ↓
JNI
          ↓
Native Library (.so)
          ↓
Linux Kernel
          ↓
Hardware
```

---

# Key Takeaways

- **Native Libraries** are precompiled **C/C++ shared libraries (`.so`)** used by Android for high-performance operations.
- They provide functionality such as graphics, media, databases, cryptography, networking, and logging.
- Java/Kotlin code communicates with native libraries through **JNI (Java Native Interface)**.
- Native libraries are stored in locations like `/system/lib`, `/system/lib64`, `/vendor/lib`, or inside an APK's `lib/` directory.
- From an Android penetration testing perspective, native libraries are a major attack surface because they are susceptible to memory corruption vulnerabilities such as **buffer overflows**, **use-after-free**, and **integer overflows**.
- Reverse engineering native libraries often involves tools like **Ghidra**, **IDA Free**, **readelf**, **objdump**, and **strings**.
