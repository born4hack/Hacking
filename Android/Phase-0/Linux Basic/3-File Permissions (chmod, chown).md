# Linux File Permissions (chmod, chown) - Android Pentesting

## What are File Permissions?

**File Permissions** determine **who can read, write, or execute** a file or directory.

Linux uses permissions to protect files from unauthorized access.

Since Android is based on the Linux Kernel, it uses the same permission model.

---

# Why are File Permissions Important?

File permissions provide:

- Access control
- Data protection
- Application isolation
- System security

For Android Pentesting, understanding file permissions helps identify:

- World-readable files
- World-writable files
- Misconfigured app data
- Privilege escalation opportunities

---

# File Ownership

Every file has:

- **Owner (User)**
- **Group**
- **Permissions**

Example:

```bash
ls -l
```

Output:

```text
-rw-r--r-- 1 alice developers 1250 report.txt
```

Explanation:

```text
Owner : alice
Group : developers
File  : report.txt
```

---

# Understanding Permission Strings

Example:

```text
-rwxr-xr--
```

Breakdown:

```text
- rwx r-x r--
│ │   │   │
│ │   │   └── Others
│ │   └────── Group
│ └────────── Owner
└──────────── File Type
```

---

# File Types

| Symbol | Meaning |
|---------|----------|
| `-` | Regular File |
| `d` | Directory |
| `l` | Symbolic Link |
| `c` | Character Device |
| `b` | Block Device |
| `s` | Socket |
| `p` | Named Pipe |

Example:

```text
drwxr-xr-x
```

The `d` means it is a directory.

---

# Permission Types

Linux has three basic permissions.

| Symbol | Name | Value |
|---------|------|-------|
| `r` | Read | 4 |
| `w` | Write | 2 |
| `x` | Execute | 1 |

---

## Read (r)

Allows reading the file.

Example:

```text
-r--------
```

User can open the file.

---

## Write (w)

Allows modifying the file.

Example:

```text
-rw-------
```

User can edit the file.

---

## Execute (x)

Allows running the file as a program.

Example:

```text
-rwx------
```

The file can be executed.

---

# Permission Categories

Permissions are divided into three groups.

```text
Owner
Group
Others
```

Example:

```text
-rwxr-xr--
```

| Category | Permissions |
|----------|-------------|
| Owner | rwx |
| Group | r-x |
| Others | r-- |

---

# Numeric Permission Values

Each permission has a numeric value.

```text
Read    = 4
Write   = 2
Execute = 1
```

Examples:

| Permission | Value |
|------------|------:|
| `rwx` | 7 |
| `rw-` | 6 |
| `r-x` | 5 |
| `r--` | 4 |
| `---` | 0 |

---

# Common Permission Values

| Numeric | Symbolic | Meaning |
|----------|----------|---------|
| 777 | `rwxrwxrwx` | Everyone has full access |
| 755 | `rwxr-xr-x` | Owner full, others read/execute |
| 700 | `rwx------` | Owner only |
| 644 | `rw-r--r--` | Owner read/write, others read |
| 600 | `rw-------` | Private file |

---

# chmod Command

`chmod` changes file permissions.

Syntax:

```bash
chmod <permissions> <file>
```

---

## Example 1

```bash
chmod 755 script.sh
```

Result:

```text
rwxr-xr-x
```

---

## Example 2

```bash
chmod 644 report.txt
```

Result:

```text
rw-r--r--
```

---

# Symbolic chmod

Permissions can also be changed symbolically.

Add execute permission:

```bash
chmod +x script.sh
```

Remove write permission:

```bash
chmod -w file.txt
```

Give owner write permission:

```bash
chmod u+w file.txt
```

Remove group read permission:

```bash
chmod g-r file.txt
```

Give others read permission:

```bash
chmod o+r file.txt
```

Symbols:

| Symbol | Meaning |
|---------|---------|
| `u` | User (Owner) |
| `g` | Group |
| `o` | Others |
| `a` | All Users |

---

# chown Command

`chown` changes the owner of a file.

Syntax:

```bash
chown <owner> <file>
```

Example:

```bash
chown alice report.txt
```

---

# Change Owner and Group

```bash
chown alice:developers report.txt
```

Now:

```text
Owner : alice
Group : developers
```

---

# chgrp Command

Changes only the group.

Example:

```bash
chgrp developers report.txt
```

---

# View File Permissions

```bash
ls -l
```

Example:

```text
-rw-r--r-- 1 alice developers report.txt
```

---

# Directory Permissions

Permissions behave slightly differently for directories.

| Permission | Meaning |
|------------|---------|
| Read | List directory contents |
| Write | Create/Delete files |
| Execute | Enter the directory (`cd`) |

Example:

```text
drwxr-xr-x
```

---

# Android File Permissions

Android uses the same Linux permission model.

Example:

```text
/data/data/com.example.app/
```

Permissions:

```text
drwx------
```

Meaning:

- Only the app owner can access the directory.
- Other apps cannot access it.

---

# Android App Sandbox

Example:

```text
App A
UID = 10234
```

Owns:

```text
/data/data/com.app.a/
```

Another app:

```text
App B
UID = 10235
```

Cannot access App A's data because the Linux Kernel enforces file permissions and UID checks.

---

# Useful Commands

View permissions:

```bash
ls -l
```

View hidden files:

```bash
ls -la
```

Change permissions:

```bash
chmod 755 file
```

Change owner:

```bash
chown user file
```

Change group:

```bash
chgrp group file
```

Display current user:

```bash
whoami
```

Display UID and GID:

```bash
id
```

---

# File Permissions from a Pentester's Perspective

Check for:

- World-readable files
- World-writable files
- Sensitive files with weak permissions
- Incorrect ownership
- Misconfigured application directories

Useful commands:

Find world-readable files:

```bash
find / -perm -004
```

Find world-writable files:

```bash
find / -perm -002
```

Find SUID files:

```bash
find / -perm -4000
```

---

# Common Security Issues

## World-Readable File

```text
-rw-r--r--
```

Sensitive data may be exposed to other users.

---

## World-Writable File

```text
-rw-rw-rw-
```

Any user can modify the file.

---

## Incorrect Ownership

Example:

```text
Owner : root
Permissions : 777
```

This is highly insecure.

---

## Weak App Directory Permissions

Example:

```text
drwxrwxrwx
```

Any application could potentially access or modify the directory.

---

# Example: Secure vs Insecure

Secure:

```text
drwx------
```

Only the owner has access.

---

Insecure:

```text
drwxrwxrwx
```

Everyone has full access.

---

# Complete Example

```text
/data/data/com.example.app/
│
├── databases/
├── files/
├── cache/
└── shared_prefs/
```

Permissions:

```text
drwx------
```

Owner:

```text
UID = 10245
```

Only the owning application (or root) can access these files.

---

# chmod vs chown

| `chmod` | `chown` |
|----------|----------|
| Changes file permissions | Changes file owner |
| Controls read/write/execute access | Changes ownership of a file or directory |
| Uses symbolic or numeric modes | Uses usernames and groups |

---

# Key Takeaways

- Linux file permissions control **who can read, write, or execute** files and directories.
- Every file has an **Owner**, **Group**, and **Permission Set**.
- The `chmod` command changes file permissions, while `chown` changes the file owner.
- Android inherits the Linux permission model to protect application data and enforce app sandboxing.
- During Android penetration testing, always check for **world-readable**, **world-writable**, or **incorrectly owned** files, as these misconfigurations can lead to **information disclosure**, **privilege escalation**, or **unauthorized access**.
