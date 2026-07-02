# Activity Manager

## Definition

The **Activity Manager** is one of Android's core system services responsible for managing the **lifecycle of applications and activities**.

It decides:

- Which app should start
- Which app should stop
- Which app stays in memory
- Which app gets killed when memory is low
- Which activity is shown on the screen

The actual system service is called the **Activity Manager Service (AMS)** and runs inside the **System Server** process.

Example:

```text
User Opens Chrome

↓

Activity Manager

↓

Start Chrome Process

↓

Show Chrome Activity
```

Without the Activity Manager, Android would not know how to launch or manage applications.

---

# Why Does Android Need Activity Manager?

Imagine opening several apps.

```text
Chrome

↓

WhatsApp

↓

Instagram

↓

YouTube
```

Android must decide:

- Which app is in the foreground
- Which apps stay in the background
- Which processes can be stopped
- How much memory each app should use

The Activity Manager handles all of these decisions.

---

# Where Does Activity Manager Fit?

The Activity Manager is a system service running inside the **System Server**.

```text
Application

↓

Binder IPC

↓

System Server

↓

Activity Manager Service (AMS)
```

Applications communicate with AMS through **Binder IPC**.

---

# Activity Manager vs Activity Manager Service (AMS)

These terms are often used interchangeably, but they are different.

## Activity Manager

The API used by applications.

Example:

```text
Application

↓

ActivityManager API
```

Apps use this API to retrieve information about running tasks, memory, and processes (subject to Android's security restrictions).

---

## Activity Manager Service (AMS)

The actual system service running inside the System Server.

Example:

```text
Application

↓

Binder

↓

Activity Manager Service
```

AMS performs all application lifecycle management.

---

# Responsibilities of Activity Manager

The Activity Manager performs many tasks.

```text
Activity Manager

├── Start Applications
├── Stop Applications
├── Launch Activities
├── Manage Tasks
├── Manage Processes
├── Manage Back Stack
├── Handle Intents
├── Monitor Memory
└── Kill Background Processes
```

---

# Step 1 — Launch an Application

Suppose the user taps Chrome.

```text
User

↓

Tap Chrome

↓

Launcher

↓

Activity Manager

↓

Start Chrome
```

AMS determines whether Chrome is already running.

---

# Starting a New Process

If the application is not already running:

```text
Activity Manager

↓

Request Zygote

↓

Fork()

↓

Chrome Process Created
```

AMS asks **Zygote** to create the application process.

---

# Starting the Main Activity

After the process is created:

```text
Chrome Process

↓

Main Activity

↓

Displayed on Screen
```

The user now sees the Chrome window.

---

# What is an Activity?

An **Activity** represents a single screen in an Android application.

Examples:

```text
Instagram Login

↓

Activity
```

```text
Instagram Feed

↓

Activity
```

```text
Instagram Profile

↓

Activity
```

Each screen is usually a separate Activity.

---

# Activity Lifecycle

The Activity Manager controls the lifecycle of every Activity.

```text
Create

↓

Start

↓

Resume

↓

Running

↓

Pause

↓

Stop

↓

Destroy
```

The lifecycle changes as the user interacts with the application.

---

# Example — Switching Apps

Suppose the user switches from Chrome to WhatsApp.

```text
Chrome

↓

Pause

↓

Stop

↓

Background
```

```text
WhatsApp

↓

Resume

↓

Foreground
```

The Activity Manager coordinates these transitions.

---

# Managing Tasks

A **Task** is a collection of related Activities.

Example:

```text
Chrome Task

├── Home Page

├── Search Results

├── Article

└── Settings
```

AMS keeps track of these tasks.

---

# The Back Stack

Android maintains a **Back Stack** for Activities.

Example:

```text
Login

↓

Dashboard

↓

Profile

↓

Settings
```

Current Stack:

```text
Top

Settings

↓

Profile

↓

Dashboard

↓

Login
```

When the user presses the Back button:

```text
Settings Removed

↓

Profile Appears
```

AMS manages this stack automatically.

---

# Managing Processes

The Activity Manager tracks all running application processes.

Example:

```text
Activity Manager

├── Chrome

├── Gmail

├── WhatsApp

├── Camera

└── Settings
```

It knows:

- Process state
- Memory usage
- Priority
- Lifecycle state

---

# Process Priority

Not all applications are equally important.

AMS assigns priorities.

```text
Foreground App

↓

Visible App

↓

Service Process

↓

Cached Process

↓

Empty Process
```

Foreground processes receive the highest priority.

---

# Memory Management

Suppose RAM becomes full.

```text
Chrome

↓

WhatsApp

↓

Instagram

↓

Camera

↓

RAM Full
```

AMS determines which process should be terminated.

Typically:

```text
Cached Process

↓

Killed First
```

Foreground applications are generally protected from being killed unless the system is under extreme memory pressure.

---

# Low Memory Handling

Example:

```text
RAM Low

↓

Activity Manager

↓

Find Lowest Priority Process

↓

Kill Process

↓

Free Memory
```

This keeps the device responsive.

---

# Handling Intents

Applications communicate using **Intents**.

Example:

```text
Gallery

↓

Share Image

↓

Intent

↓

WhatsApp
```

AMS decides which application should receive the Intent.

---

# Launch Modes

AMS respects Activity launch modes defined in the manifest.

Examples include:

- Standard
- SingleTop
- SingleTask
- SingleInstance

These determine how Activities are created and reused.

---

# Activity Manager and Binder

Applications communicate with AMS using Binder IPC.

Example:

```text
Application

↓

Binder

↓

Activity Manager

↓

Response
```

Applications never communicate with AMS directly.

---

# Activity Manager and Security

AMS performs several security-related checks.

Examples include:

- Permission enforcement
- Activity launch validation
- Intent validation
- Task isolation
- User profile separation

Example:

```text
Application

↓

Launch Activity

↓

Permission Check

↓

Allowed?
```

---

# Activity Manager from a Pentester's Perspective

Activity Manager is closely related to many Android attack surfaces.

Security topics include:

- Activity Hijacking
- Task Hijacking
- Intent Injection
- Exported Activities
- Task Affinity abuse
- Activity Spoofing
- Launch Mode vulnerabilities
- Privilege Escalation

Many Android vulnerabilities involve improperly exposed Activities or unsafe Intent handling.

---

# Real-World Example

Suppose a user opens a banking application.

```text
User

↓

Tap Banking App

↓

Activity Manager

↓

Request Zygote

↓

Create Process

↓

Launch Login Activity

↓

Display Screen
```

Now consider another scenario.

A malicious application sends an Intent to an exported Activity.

```text
Malicious App

↓

Intent

↓

Activity Manager

↓

Exported Activity

↓

Sensitive Function Executed
```

If the exported Activity does not validate the caller or input correctly, an attacker may trigger unauthorized functionality.

---

# Key Terms

| Term | Meaning |
|------|---------|
| Activity | A single screen in an Android application |
| Activity Manager | API used to interact with application and task information |
| Activity Manager Service (AMS) | System service that manages application and activity lifecycles |
| Task | A collection of related Activities |
| Back Stack | Stack of Activities used for navigation |
| Foreground Process | Process currently interacting with the user |
| Background Process | Process not currently visible to the user |
| Cached Process | Inactive process kept in memory for faster startup |
| Intent | Messaging object used to request actions between Android components |
| Launch Mode | Rule that controls how Activities are created and reused |
| Binder IPC | Communication mechanism between applications and AMS |

---

# Key Takeaways

- The Activity Manager Service (AMS) is responsible for managing Android applications, Activities, tasks, and processes.
- It starts new application processes by requesting Zygote to fork a new process.
- AMS controls the Activity lifecycle, including creation, pausing, resuming, stopping, and destruction.
- It manages the Back Stack and task organization to provide smooth navigation.
- AMS assigns process priorities and manages memory by terminating lower-priority processes when needed.
- Applications communicate with AMS through Binder IPC.
- Understanding the Activity Manager is essential for learning Activity lifecycle, Intents, task management, and Android penetration testing topics such as Activity Hijacking and Intent Injection.
