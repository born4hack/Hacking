# Linux Users & Groups (Android Pentesting)

## What are Users and Groups?

Linux is a **multi-user operating system**, meaning multiple users can use the same system while remaining isolated from each other.

Every process, file, and application belongs to a **User** and one or more **Groups**.

Android inherits this model from Linux and uses **Linux Users (UIDs)** and **Groups (GIDs)** to isolate applications and enforce security.

---

# Why are Users & Groups Important?

Users and Groups provide:

- Access control
- File ownership
- Process isolation
- Security
- Permission management

In Android, every application is assigned a **unique Linux User ID (UID)**, preventing apps from accessing each other's private data.

---

# User vs Group

| User | Group |
|------|-------|
| Represents an individual account | Represents a collection of users |
| Owns files and processes | Defines shared permissions |
| Has a unique UID | Has a unique GID (Group ID) |

Example:

```text
User: alice
UID: 1001

Groups:
developers
docker
sudo
```

---

# What is a UID?

A **UID (User ID)** is a unique number assigned to every user.

Example:

```text
root
UID = 0

alice
UID = 1000

bob
UID = 1001
```

Linux uses the UID—not the username—to determine permissions.

View your UID:

```bash
id
```

Example output:

```text
uid=1000(alice)
gid=1000(alice)
groups=1000(alice),27(sudo),999(docker)
```

---

# What is a GID?

A **GID (Group ID)** is a unique number assigned to a group.

Example:

```text
developers
GID = 1001

docker
GID = 999
```

A user can belong to multiple groups.

---

# Types of Users

## 1. Root User

The **root** user is the administrator.

Characteristics:

- UID = 0
- Full system access
- Can read, modify, or delete any file
- Can manage users and processes

Example:

```text
Username: root
UID: 0
```

---

## 2. Normal User

A regular user with limited permissions.

Example:

```text
Username: alice
UID: 1000
```

Normal users cannot modify protected system files.

---

## 3. System Users

Used by services and daemons.

Examples:

```text
www-data
mysql
daemon
systemd-network
```

These users are generally not used for interactive logins.

---

# Groups

Groups make it easier to manage permissions for multiple users.

Example:

```text
developers

alice
bob
charlie
```

Instead of assigning permissions individually, you assign them to the group.

---

# File Ownership

Every file has:

- Owner (User)
- Group

Example:

```bash
ls -l
```

Output:

```text
-rw-r--r-- 1 alice developers 1200 report.txt
```

Meaning:

- Owner: `alice`
- Group: `developers`

---

# View Current User

```bash
whoami
```

Example output:

```text
alice
```

---

# View User Information

```bash
id
```

Example:

```text
uid=1000(alice)
gid=1000(alice)
groups=1000(alice),27(sudo)
```

---

# View Groups

```bash
groups
```

Example:

```text
alice sudo docker
```

---

# Important Linux User Files

## `/etc/passwd`

Stores user account information.

Example:

```text
root:x:0:0:root:/root:/bin/bash
```

Fields:

```text
Username
Password Placeholder
UID
GID
Description
Home Directory
Login Shell
```

View:

```bash
cat /etc/passwd
```

---

## `/etc/group`

Stores group information.

Example:

```text
developers:x:1001:alice,bob
```

View:

```bash
cat /etc/group
```

---

## `/etc/shadow`

Stores password hashes.

Example:

```text
root:$6$...
```

Only the root user can read this file.

---

# Android Users & Groups

Android does **not** create a normal Linux user account for every app.

Instead, every application receives its own **Linux UID**.

Example:

```text
Instagram
UID = 10234

WhatsApp
UID = 10235

Chrome
UID = 10236
```

Each app runs as a different Linux user.

---

# Android Application Isolation

Example:

```text
Instagram
UID = 10234

WhatsApp
UID = 10235
```

Instagram **cannot** access:

```text
/data/data/com.whatsapp/
```

Because the Linux Kernel checks the UID before allowing access.

---

# Android Security Model

```text
App A
UID 10234
      │
      │
      ├── Own Files
      │
      └── Own Process

App B
UID 10235
      │
      ├── Own Files
      │
      └── Own Process
```

Apps are isolated by default.

---

# Shared UID (Legacy)

Older Android versions allowed multiple apps signed with the same certificate to share a UID.

Example:

```xml
android:sharedUserId="com.example.shared"
```

This feature is deprecated and should not be used in new applications.

---

# Linux Users in Android

Examples:

```text
root
system
shell
radio
bluetooth
nobody
```

Each system service runs under a specific Linux user.

---

# Useful Commands

Current user:

```bash
whoami
```

Current UID and groups:

```bash
id
```

Current groups:

```bash
groups
```

List users:

```bash
cat /etc/passwd
```

List groups:

```bash
cat /etc/group
```

---

# Users & Groups from a Pentester's Perspective

When assessing Android applications, understand:

- Which UID the app runs as
- Which files it owns
- Which permissions it has
- Which groups it belongs to
- Whether apps can improperly access another app's data

---

# Common Security Issues

## Incorrect File Ownership

Example:

```text
Sensitive File
Owner: root
Permissions: world-readable
```

This may expose confidential information.

---

## Incorrect Group Permissions

Example:

```text
App Data
Group: everyone
```

Other users or processes may gain unintended access.

---

## Privilege Escalation

```text
Normal App
      ↓
Privilege Escalation
      ↓
Root User
```

If an attacker gains **UID 0**, they effectively have full control over the device.

---

# Example: Android App Directory

```text
/data/data/com.example.app/
```

Owner:

```text
UID = 10245
```

Only this UID (and root) can normally access the directory.

---

# Complete Flow

```text
Install App
      ↓
Android Package Manager
      ↓
Assign Unique UID
      ↓
Create App Directory
      ↓
Set File Ownership
      ↓
App Runs in Its Own Sandbox
```

---

# Users & Groups vs Android Sandbox

| Linux Users & Groups | Android Sandbox |
|-----------------------|-----------------|
| Uses UIDs and GIDs | Built on Linux UIDs |
| Controls file ownership | Isolates each app |
| Controls process permissions | Prevents cross-app access |
| Core Linux security feature | Android security model built on Linux |

---

# Key Takeaways

- Linux uses **Users (UIDs)** and **Groups (GIDs)** to manage ownership, permissions, and access control.
- Every file and process belongs to a user and a group.
- The **root** user (UID `0`) has unrestricted access to the system.
- Android assigns **each application a unique Linux UID**, creating an isolated sandbox for every app.
- The Linux Kernel enforces this isolation, preventing apps from directly accessing each other's private files and processes.
- Understanding Users & Groups is essential for Android penetration testing because it explains how Android enforces application isolation and how privilege escalation vulnerabilities can break that security model.
