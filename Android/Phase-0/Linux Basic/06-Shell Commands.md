# Linux Shell Commands - Android Pentesting

## What is a Shell?

A **Shell** is a command-line interface (CLI) that allows users to interact with the Linux operating system.

Instead of clicking icons (GUI), you type commands into the shell.

The shell receives your commands, passes them to the Linux Kernel, and displays the results.

---

# Why is the Shell Important?

The shell allows you to:

- Navigate the file system
- Manage files
- Manage processes
- View logs
- Search for files
- Execute programs
- Automate tasks

For Android Pentesting, most activities are performed using a Linux shell and the Android Debug Bridge (ADB).

---

# Shell Architecture

```text
User
    ↓
Shell (Bash/Zsh)
    ↓
Linux Kernel
    ↓
Hardware
```

---

# Common Linux Shells

| Shell | Description |
|--------|-------------|
| Bash | Most common Linux shell |
| Zsh | Advanced Bash alternative |
| Sh | Original Unix shell |
| Fish | User-friendly shell |

Most Linux distributions use **Bash** by default.

---

# Open a Terminal

Common shortcuts:

```text
Ctrl + Alt + T
```

Check the current shell:

```bash
echo $SHELL
```

Example output:

```text
/bin/bash
```

---

# Current Working Directory

Display your current location:

```bash
pwd
```

Example:

```text
/home/alice
```

---

# List Files

```bash
ls
```

Example:

```text
Desktop
Documents
Downloads
```

---

# Detailed Listing

```bash
ls -l
```

Example:

```text
-rw-r--r-- report.txt
drwxr-xr-x Downloads
```

---

# Show Hidden Files

```bash
ls -la
```

Hidden files begin with:

```text
.
```

Example:

```text
.bashrc
.git
.profile
```

---

# Change Directory

```bash
cd Documents
```

Go to the parent directory:

```bash
cd ..
```

Go to your home directory:

```bash
cd
```

Go to the root directory:

```bash
cd /
```

---

# Create Directory

```bash
mkdir test
```

Create nested directories:

```bash
mkdir -p test/demo/example
```

---

# Remove Directory

Empty directory:

```bash
rmdir test
```

Directory with files:

```bash
rm -r test
```

---

# Create File

```bash
touch notes.txt
```

---

# View File Contents

```bash
cat notes.txt
```

View page by page:

```bash
less notes.txt
```

View the beginning of a file:

```bash
head notes.txt
```

View the end of a file:

```bash
tail notes.txt
```

Follow a growing log file:

```bash
tail -f app.log
```

---

# Copy Files

```bash
cp file1.txt file2.txt
```

Copy directories:

```bash
cp -r folder backup
```

---

# Move or Rename Files

Rename:

```bash
mv old.txt new.txt
```

Move:

```bash
mv file.txt Documents/
```

---

# Delete Files

```bash
rm file.txt
```

Delete directories recursively:

```bash
rm -r folder
```

Force delete:

```bash
rm -rf folder
```

> **Warning:** `rm -rf` permanently deletes files and directories.

---

# Search for Files

```bash
find / -name "*.apk"
```

Example:

```bash
find /data -name "*.db"
```

---

# Search Inside Files

```bash
grep password config.txt
```

Recursive search:

```bash
grep -R "token" .
```

---

# Count Lines, Words, and Characters

```bash
wc file.txt
```

---

# Display File Type

```bash
file app.apk
```

---

# Show Disk Usage

```bash
df -h
```

Directory size:

```bash
du -sh Downloads
```

---

# View Running Processes

```bash
ps
```

All processes:

```bash
ps -A
```

Interactive process viewer:

```bash
top
```

---

# Kill a Process

```bash
kill PID
```

Force kill:

```bash
kill -9 PID
```

---

# Show Current User

```bash
whoami
```

---

# Show User Information

```bash
id
```

---

# Display Command History

```bash
history
```

Run the previous command:

```bash
!!
```

---

# Clear the Terminal

```bash
clear
```

Shortcut:

```text
Ctrl + L
```

---

# Pipes (`|`)

Pass the output of one command to another.

Example:

```bash
ps -A | grep chrome
```

Flow:

```text
ps -A
    ↓
Pipe
    ↓
grep chrome
```

---

# Redirection

Save output to a file:

```bash
ls > files.txt
```

Append output:

```bash
ls >> files.txt
```

Read input from a file:

```bash
sort < names.txt
```

---

# Wildcards

List all APK files:

```bash
ls *.apk
```

Find all databases:

```bash
find . -name "*.db"
```

---

# Command Chaining

Run the second command only if the first succeeds:

```bash
mkdir test && cd test
```

Run the second command regardless of success:

```bash
mkdir test ; cd test
```

---

# Environment Variables

View all variables:

```bash
env
```

Display a specific variable:

```bash
echo $HOME
```

---

# Android Shell

Open an Android shell using ADB:

```bash
adb shell
```

Example prompt:

```text
generic_x86:/ $
```

---

# Useful Android Shell Commands

List installed packages:

```bash
pm list packages
```

Find an application's APK:

```bash
pm path com.example.app
```

List running processes:

```bash
ps -A
```

View logs:

```bash
logcat
```

List files:

```bash
ls
```

Navigate directories:

```bash
cd /data/data
```

Check Android version:

```bash
getprop ro.build.version.release
```

Check device information:

```bash
getprop
```

---

# Shell Commands from a Pentester's Perspective

During Android penetration testing, shell commands are used to:

- Navigate application directories
- Locate APK files
- View databases
- Inspect SharedPreferences
- Capture logs
- Enumerate installed packages
- Find native libraries
- Inspect running processes
- Gather system information

---

# Common Pentesting Commands

Find APK files:

```bash
find / -name "*.apk"
```

Find databases:

```bash
find /data -name "*.db"
```

Find SharedPreferences:

```bash
find /data/data -name "*.xml"
```

Find native libraries:

```bash
find / -name "*.so"
```

List installed packages:

```bash
pm list packages
```

View logs:

```bash
logcat
```

Inspect memory usage:

```bash
dumpsys meminfo com.example.app
```

---

# Example Android Investigation

```text
ADB Shell
      ↓
Locate APK
      ↓
Find Application Data
      ↓
Inspect Database
      ↓
Read SharedPreferences
      ↓
Analyze Logs
```

---

# Most Important Commands for Android Pentesting

| Command | Purpose |
|----------|---------|
| `pwd` | Show current directory |
| `ls -la` | List files with details |
| `cd` | Change directory |
| `cat` | Read a file |
| `find` | Search for files |
| `grep` | Search text |
| `ps -A` | List running processes |
| `top` | Monitor processes |
| `chmod` | Change permissions |
| `chown` | Change owner |
| `logcat` | View Android logs |
| `adb shell` | Open Android shell |
| `pm` | Manage installed packages |
| `dumpsys` | Inspect Android system services |
| `getprop` | Display system properties |

---

# Key Takeaways

- A **Shell** is a command-line interface that allows users to interact with Linux by executing commands.
- Shell commands are essential for navigating the file system, managing processes, searching files, and automating tasks.
- Android penetration testers frequently use **ADB Shell** to interact with Android devices, inspect application data, analyze logs, and gather system information.
- Mastering common Linux and Android shell commands is a fundamental skill for effective Android application security testing.
