# Linux Environment Variables - Android Pentesting

## What are Environment Variables?

**Environment Variables** are **named key-value pairs** that store information used by the operating system, shell, and applications.

They define the environment in which a program runs.

Think of them as **configuration settings** that programs can read while executing.

Example:

```text
HOME=/home/usman
```

Here:

- Variable Name: `HOME`
- Value: `/home/usman`

---

# Why are Environment Variables Important?

Environment variables help:

- Store configuration values
- Locate executables
- Define user information
- Configure applications
- Control program behavior

For Android Pentesting, they are commonly used to configure:

- Android SDK
- Java
- Gradle
- ADB
- Emulator
- Frida
- Development tools

---

# How Environment Variables Work

```text
User
   │
   ▼
Shell
   │
Environment Variables
   │
   ▼
Application
```

When a program starts, it inherits the environment variables from its parent process.

---

# Variable Format

```text
NAME=VALUE
```

Example:

```text
HOME=/home/usman
```

---

# Common Environment Variables

| Variable | Purpose |
|----------|---------|
| `HOME` | User's home directory |
| `PATH` | Directories searched for executable programs |
| `USER` | Current username |
| `SHELL` | Current shell |
| `PWD` | Current working directory |
| `OLDPWD` | Previous working directory |
| `HOSTNAME` | System hostname |
| `LANG` | System language and locale |
| `TERM` | Terminal type |

---

# View All Environment Variables

```bash
env
```

or

```bash
printenv
```

Example output:

```text
HOME=/home/usman
USER=usman
PATH=/usr/bin:/bin
```

---

# View a Specific Variable

Syntax:

```bash
echo $VARIABLE_NAME
```

Examples:

```bash
echo $HOME
```

Output:

```text
/home/usman
```

Current user:

```bash
echo $USER
```

Current shell:

```bash
echo $SHELL
```

Current directory:

```bash
echo $PWD
```

---

# PATH Variable

`PATH` is one of the most important environment variables.

It tells Linux **where to look for executable programs**.

Example:

```bash
echo $PATH
```

Output:

```text
/usr/local/bin:/usr/bin:/bin
```

Flow:

```text
User types:

adb

        │
        ▼

Search PATH

        │
        ▼

/usr/local/bin
/usr/bin
/bin

        │
        ▼

Execute adb
```

If the executable is not found in one of these directories, Linux returns:

```text
command not found
```

---

# Creating an Environment Variable

Temporary variable:

```bash
export NAME="Usman"
```

Check it:

```bash
echo $NAME
```

Output:

```text
Usman
```

This variable exists only for the current terminal session.

---

# Removing a Variable

```bash
unset NAME
```

---

# Persistent Environment Variables

To make variables available every time you open a terminal, add them to:

```text
~/.bashrc
```

or

```text
~/.profile
```

Example:

```bash
export NAME="Usman"
```

Reload the file:

```bash
source ~/.bashrc
```

---

# Android Development Variables

Android development commonly uses these variables:

## ANDROID_HOME

Points to the Android SDK.

Example:

```bash
export ANDROID_HOME=$HOME/Android/Sdk
```

---

## ANDROID_SDK_ROOT

Another variable pointing to the Android SDK.

Example:

```bash
export ANDROID_SDK_ROOT=$HOME/Android/Sdk
```

---

## JAVA_HOME

Points to the Java installation.

Example:

```bash
export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64
```

---

## PATH with Android SDK

Example:

```bash
export PATH=$PATH:$ANDROID_HOME/platform-tools
```

This allows you to run:

```bash
adb
```

instead of:

```bash
/home/usman/Android/Sdk/platform-tools/adb
```

---

# Check ADB

```bash
which adb
```

Example output:

```text
/usr/bin/adb
```

or

```text
/home/usman/Android/Sdk/platform-tools/adb
```

---

# Check Java

```bash
which java
```

---

# View All Shell Variables

```bash
set
```

---

# Useful Commands

Show all variables:

```bash
env
```

Show one variable:

```bash
echo $PATH
```

Find a variable:

```bash
printenv | grep JAVA
```

Show executable location:

```bash
which adb
```

---

# Android Pentesting Example

Suppose you type:

```bash
adb devices
```

Linux performs:

```text
PATH
     │
Search directories
     │
Find adb
     │
Execute adb
```

If `adb` is not in the `PATH`, you'll see:

```text
adb: command not found
```

---

# Frida Example

After installing Frida:

```bash
frida-ps -U
```

Linux searches:

```text
PATH
    │
Find frida-ps
    │
Execute Program
```

If Frida's installation directory isn't in `PATH`, the command will fail.

---

# Environment Variables from a Pentester's Perspective

Environment variables help configure and automate tools such as:

- Android SDK
- ADB
- Fastboot
- Java
- Gradle
- Frida
- Emulator

They also help scripts locate executables without using full paths.

---

# Common Problems

## PATH Not Configured

Example:

```text
adb: command not found
```

Cause:

```text
ADB directory is missing from PATH.
```

---

## Incorrect JAVA_HOME

Example:

```text
Gradle build failed
```

Cause:

```text
JAVA_HOME points to the wrong Java installation.
```

---

## Missing ANDROID_HOME

Example:

```text
SDK not found
```

Cause:

```text
Android SDK location is not configured.
```

---

# Complete Flow

```text
User Types Command
        │
        ▼
Shell
        │
Reads PATH
        │
Searches Directories
        │
Finds Executable
        │
Runs Program
```

---

# Common Environment Variables for Android

| Variable | Purpose |
|----------|---------|
| `PATH` | Finds executable programs |
| `HOME` | User's home directory |
| `PWD` | Current directory |
| `SHELL` | Current shell |
| `USER` | Logged-in user |
| `ANDROID_HOME` | Android SDK location |
| `ANDROID_SDK_ROOT` | Android SDK location |
| `JAVA_HOME` | Java installation |
| `GRADLE_HOME` | Gradle installation (if configured) |

---

# Key Takeaways

- **Environment Variables** are key-value pairs that configure the operating system, shell, and applications.
- The **`PATH`** variable is one of the most important because it tells Linux where to find executable programs.
- Variables can be **temporary** (current shell only) or **persistent** (stored in files like `~/.bashrc`).
- Android development and penetration testing commonly rely on variables such as **`ANDROID_HOME`**, **`ANDROID_SDK_ROOT`**, **`JAVA_HOME`**, and **`PATH`**.
- Correctly configuring environment variables ensures tools like **ADB**, **Fastboot**, **Gradle**, and **Frida** work properly without requiring full file paths.
