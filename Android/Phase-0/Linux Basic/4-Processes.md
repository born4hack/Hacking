# Linux Processes - Android Pentesting

## What is a Process?

A **Process** is a **running instance of a program**.

When you execute a program, Linux loads it into memory and starts executing it. This running program is called a **process**.

Examples:

- Opening Chrome creates a Chrome process.
- Running a Python script creates a Python process.
- Starting an Android app creates an Android application process.

---

# Program vs Process

| Program | Process |
|----------|----------|
| Stored on disk | Running in memory |
| Passive | Active |
| Static | Dynamic |
| Executable file | Executing program |

Example:

```text
chrome (Program)
        ↓
User opens Chrome
        ↓
Chrome Process
```

---

# Why are Processes Important?

Processes allow Linux to:

- Run multiple applications simultaneously
- Isolate applications
- Allocate CPU time
- Manage memory
- Enforce security

Android inherits this process model from Linux.

---

# Process Lifecycle

```text
Program
    ↓
Loaded into Memory
    ↓
Process Created
    ↓
Running
    ↓
Waiting
    ↓
Terminated
```

---

# Process Components

Every process has:

- Process ID (PID)
- Parent Process ID (PPID)
- User ID (UID)
- Group ID (GID)
- Memory
- Threads
- Open Files
- Environment Variables

---

# Process ID (PID)

Every process has a unique **Process ID (PID)**.

Example:

```text
Chrome
PID = 2450

Firefox
PID = 3121

Python
PID = 4580
```

View processes:

```bash
ps
```

---

# Parent Process (PPID)

Every process is created by another process.

Example:

```text
systemd
   │
   ├── bash
   │      │
   │      └── python
```

Example:

```text
systemd
PID = 1

bash
PID = 430

python
PID = 440
```

Python's parent is Bash.

---

# Process States

Linux processes can be in different states.

| State | Meaning |
|---------|----------|
| Running | Currently executing |
| Sleeping | Waiting for an event |
| Stopped | Temporarily paused |
| Zombie | Finished but not cleaned up |
| Dead | Terminated |

---

# Process Scheduling

The Linux Kernel decides which process gets CPU time.

Example:

```text
Chrome
        ↓
Kernel Scheduler
        ↓
CPU

Firefox
        ↓
Kernel Scheduler
        ↓
CPU
```

This allows multiple applications to run seemingly at the same time.

---

# Foreground vs Background Processes

## Foreground Process

Runs while interacting with the user.

Example:

```bash
nano notes.txt
```

---

## Background Process

Runs without user interaction.

Example:

```bash
python server.py &
```

The `&` starts the process in the background.

---

# Viewing Processes

Show current processes:

```bash
ps
```

Show all processes:

```bash
ps -A
```

Detailed view:

```bash
ps -ef
```

Interactive process viewer:

```bash
top
```

Modern alternative:

```bash
htop
```

---

# Finding a Process

Find Chrome:

```bash
ps -A | grep chrome
```

Find a process by name:

```bash
pidof chrome
```

---

# Killing a Process

Terminate a process:

```bash
kill PID
```

Example:

```bash
kill 2450
```

Force terminate:

```bash
kill -9 2450
```

---

# Process Priority

Linux assigns a priority to each process.

Lower **nice** value:

- Higher priority

Higher **nice** value:

- Lower priority

View priorities:

```bash
top
```

Change priority:

```bash
nice
```

---

# Process Memory

Each process has its own memory space.

Example:

```text
Chrome
Memory

Firefox
Memory

Python
Memory
```

Processes cannot normally access each other's memory.

---

# Threads

A process can contain multiple **threads**.

Example:

```text
Chrome Process
      │
      ├── UI Thread
      ├── Network Thread
      ├── Render Thread
      └── GPU Thread
```

Threads share the same process memory.

---

# Android Processes

Every Android application runs as a **separate Linux process**.

Example:

```text
WhatsApp
PID = 2134

Instagram
PID = 2145

Chrome
PID = 2158
```

Each application has:

- Its own PID
- Its own UID
- Its own memory
- Its own sandbox

---

# Android Process Creation

```text
User Opens App
        ↓
Zygote
        ↓
Fork()
        ↓
New Application Process
        ↓
App Starts
```

Android uses the **Zygote** process to quickly create new app processes.

---

# Android Sandbox

Example:

```text
App A
PID = 2010
UID = 10234

App B
PID = 2015
UID = 10235
```

Each app:

- Runs independently
- Has isolated memory
- Has isolated files
- Cannot directly access another app's process

---

# Android Process Types

Examples:

```text
Foreground App

Background App

Cached App

System Process

Service Process
```

Android changes process priority based on importance to optimize memory usage.

---

# Useful Commands (Linux)

Show all processes:

```bash
ps -A
```

Detailed process list:

```bash
ps -ef
```

Interactive viewer:

```bash
top
```

Find a process:

```bash
pidof bash
```

Kill a process:

```bash
kill PID
```

Force kill:

```bash
kill -9 PID
```

---

# Useful Commands (Android)

Open ADB shell:

```bash
adb shell
```

List running processes:

```bash
ps -A
```

Find an app process:

```bash
pidof com.example.app
```

View memory usage:

```bash
dumpsys meminfo com.example.app
```

Kill an app:

```bash
am force-stop com.example.app
```

---

# Processes from a Pentester's Perspective

During Android penetration testing, you often inspect running processes to:

- Identify the target application's PID
- Attach debugging or instrumentation tools
- Monitor memory usage
- Analyze runtime behavior
- Hook methods with Frida

Example:

```text
Target App
      ↓
Find PID
      ↓
Attach Frida
      ↓
Hook Methods
```

---

# Common Security Issues

## Debuggable Process

A debuggable application may allow:

- Runtime inspection
- Memory analysis
- Method hooking

---

## Sensitive Data in Memory

Applications may store:

- Passwords
- API Tokens
- Session Tokens
- Encryption Keys

If memory is not handled securely, sensitive information may be exposed.

---

## Privilege Escalation

```text
Normal App
      ↓
Kernel Vulnerability
      ↓
Root Process
```

A successful privilege escalation may allow an attacker to execute code with elevated privileges.

---

# Complete Flow

```text
User Opens App
        ↓
Launcher
        ↓
Zygote
        ↓
fork()
        ↓
New Linux Process
        ↓
ART Starts
        ↓
Application Runs
```

---

# Program vs Process vs Thread

| Program | Process | Thread |
|----------|----------|---------|
| File stored on disk | Running instance of a program | Smallest unit of execution inside a process |
| Passive | Active | Shares memory with other threads in the same process |
| Executable | Has PID, memory, and resources | Has Thread ID (TID) |

---

# Key Takeaways

- A **process** is a running instance of a program.
- Every process has a unique **PID (Process ID)** and is managed by the Linux Kernel.
- Processes have their own memory, resources, and execution context.
- Android runs **each application in its own Linux process** with a unique **UID**, providing strong process isolation.
- Android creates application processes by **forking the Zygote process**, improving startup performance.
- Understanding Linux processes is essential for Android penetration testing because tools such as **ADB**, **Frida**, and debuggers interact directly with running application processes.
