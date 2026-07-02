# Linux File System (Android Pentesting)

## What is a Linux File System?

A **Linux File System** is the way Linux organizes, stores, and manages files and directories.

Everything in Linux is treated as a **file**, including:

- Regular files
- Directories
- Hard disks
- USB devices
- Network interfaces
- Processes
- System information

Android is built on the **Linux Kernel**, so it follows a Linux-based file system structure.

---

# Why is the Linux File System Important?

The Linux File System provides:

- Organized storage
- Security through permissions
- Easy file management
- Device management
- Process information
- Configuration storage

For Android Pentesting, understanding the file system helps you locate:

- APK files
- App data
- Databases
- Logs
- Native libraries
- Configuration files
- Sensitive information

---

# Linux File System Hierarchy

Linux starts from a single directory called the **Root Directory**.

```text
/
├── bin
├── boot
├── dev
├── etc
├── home
├── lib
├── media
├── mnt
├── opt
├── proc
├── root
├── run
├── sbin
├── srv
├── sys
├── tmp
├── usr
└── var
```

Everything starts from:

```text
/
```

This is called the **Root Directory**.

---

# Important Linux Directories

## /

The root directory.

Example:

```text
/
```

All other directories are inside it.

---

## /bin

Contains essential user commands.

Examples:

```bash
ls
cp
mv
cat
echo
pwd
rm
```

Example:

```bash
ls /bin
```

---

## /sbin

Contains system administration commands.

Examples:

```bash
fsck
mount
reboot
shutdown
```

Usually used by the root user.

---

## /boot

Contains files required to boot Linux.

Examples:

```text
Kernel Image
Boot Loader Files
initramfs
```

Android has a similar concept with:

- Boot Image
- Bootloader

---

## /dev

Contains device files.

Everything connected to Linux appears as a file.

Examples:

```text
/dev/null
/dev/random
/dev/zero
/dev/sda
/dev/tty
```

Android examples:

```text
/dev/socket
/dev/binder
```

---

## /etc

Contains system configuration files.

Examples:

```text
hosts
passwd
resolv.conf
```

---

## /home

Contains home directories for users.

Example:

```text
/home/alice
/home/bob
```

Android does not typically use `/home` for application data.

---

## /root

Home directory of the root user.

Example:

```text
/root
```

---

## /tmp

Temporary files.

Applications often store temporary data here.

Example:

```text
/tmp
```

---

## /usr

Contains user programs and libraries.

Examples:

```text
/ usr/bin
/usr/lib
/usr/share
```

---

## /var

Contains variable data.

Examples:

```text
Logs
Cache
Mail
Databases
```

---

## /proc

A **virtual filesystem**.

It does **not** store files on disk.

Instead, it provides runtime information about:

- Processes
- CPU
- Memory
- Kernel

Example:

```bash
ls /proc
```

Useful files:

```text
/proc/cpuinfo
/proc/meminfo
/proc/version
```

Example:

```bash
cat /proc/version
```

---

## /sys

Another virtual filesystem.

Provides information about:

- Hardware
- Drivers
- Kernel objects

Example:

```bash
ls /sys
```

---

# Android-Specific Directories

Android extends the Linux file system with additional directories.

```text
/
├── system
├── data
├── vendor
├── product
├── storage
├── sdcard
├── cache
└── metadata
```

---

## /system

Contains Android system files.

Examples:

```text
Framework
System Apps
Libraries
Fonts
```

---

## /data

One of the most important directories.

Contains user-installed applications and their data.

Example:

```text
/data/app/
/data/data/
```

---

### /data/app

Stores installed APKs.

Example:

```text
/data/app/com.example.app/
```

---

### /data/data

Stores private application data.

Example:

```text
/data/data/com.example.app/
```

Contains:

```text
databases/
shared_prefs/
cache/
files/
```

---

## /vendor

Contains manufacturer-specific files.

Examples:

```text
Camera HAL
Bluetooth HAL
Drivers
Native Libraries
```

---

## /product

Contains product-specific applications and configurations.

---

## /storage

Contains mounted storage devices.

Example:

```text
/storage/emulated/0
```

---

## /sdcard

Represents external/shared storage.

Example:

```text
/sdcard/Download
```

---

# File Types in Linux

| Symbol | File Type |
|---------|-----------|
| `-` | Regular File |
| `d` | Directory |
| `l` | Symbolic Link |
| `c` | Character Device |
| `b` | Block Device |
| `s` | Socket |
| `p` | Named Pipe |

Example:

```bash
ls -l
```

Output:

```text
drwxr-xr-x
-rw-r--r--
lrwxrwxrwx
```

---

# Absolute vs Relative Paths

## Absolute Path

Starts from `/`.

Example:

```text
/data/data/com.example.app
```

---

## Relative Path

Starts from the current directory.

Example:

```text
Documents/report.txt
```

---

# Useful Linux Commands

Current directory:

```bash
pwd
```

List files:

```bash
ls
```

Detailed listing:

```bash
ls -la
```

Change directory:

```bash
cd
```

Create directory:

```bash
mkdir test
```

Create file:

```bash
touch file.txt
```

Copy file:

```bash
cp file1 file2
```

Move file:

```bash
mv old new
```

Delete file:

```bash
rm file.txt
```

Delete directory:

```bash
rm -r test
```

Read file:

```bash
cat file.txt
```

Search for files:

```bash
find / -name "*.apk"
```

Search text inside files:

```bash
grep password file.txt
```

---

# Linux File System from a Pentester's Perspective

A pentester frequently explores the file system to find:

- APK files
- SQLite databases
- SharedPreferences
- Logs
- Configuration files
- API keys
- Tokens
- Native libraries
- Backup files

Example:

```bash
find /data -name "*.db"
```

Find APK files:

```bash
find / -name "*.apk"
```

Find shared preferences:

```bash
find /data/data -name "*.xml"
```

Find native libraries:

```bash
find / -name "*.so"
```

---

# Example: Android Application Directory

```text
/data/data/com.example.app/
├── cache/
├── code_cache/
├── databases/
├── files/
├── no_backup/
└── shared_prefs/
```

---

# Complete Example

```text
/
│
├── system
├── data
│   ├── app
│   └── data
│       └── com.example.app
│           ├── cache
│           ├── databases
│           ├── files
│           └── shared_prefs
│
├── vendor
├── storage
└── sdcard
```

---

# Key Takeaways

- The **Linux File System** organizes all files and directories under a single **Root Directory (`/`)**.
- In Linux, **everything is treated as a file**, including devices, processes, and system information.
- Android inherits a Linux-based file system and adds directories such as `/system`, `/data`, `/vendor`, and `/storage`.
- The most important directory for Android penetration testing is **`/data/data/<package_name>/`**, which stores an application's private data.
- Understanding the Linux file system is essential for locating APKs, databases, SharedPreferences, logs, native libraries, and other sensitive files during Android application security assessments.
