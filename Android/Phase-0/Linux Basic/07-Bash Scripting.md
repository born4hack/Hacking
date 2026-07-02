# Bash Scripting - Android Pentesting

## What is Bash Scripting?

**Bash Scripting** is the process of writing a sequence of Linux shell commands into a file so they can be executed automatically.

Instead of typing commands one by one, you write them in a **script**, making repetitive tasks faster and less error-prone.

A Bash script is simply a text file containing Bash commands.

Example:

```bash
#!/bin/bash

echo "Hello, World!"
```

---

# Why Learn Bash Scripting?

Bash scripting helps you:

- Automate repetitive tasks
- Save time
- Reduce human errors
- Perform bulk operations
- Create custom security tools
- Automate Android pentesting workflows

---

# Why is Bash Important for Android Pentesting?

During Android security testing, you often repeat tasks such as:

- Pulling APKs
- Collecting logs
- Installing applications
- Running ADB commands
- Capturing screenshots
- Starting Frida
- Extracting databases

Instead of executing dozens of commands manually, you can automate them with Bash scripts.

---

# Bash Script Structure

Every Bash script starts with a **Shebang**.

```bash
#!/bin/bash
```

This tells Linux to execute the script using the Bash interpreter.

Example:

```bash
#!/bin/bash

echo "Android Pentesting"
```

---

# Creating a Bash Script

Create a file:

```bash
touch hello.sh
```

Edit it:

```bash
nano hello.sh
```

Add:

```bash
#!/bin/bash

echo "Hello"
```

Save the file.

Make it executable:

```bash
chmod +x hello.sh
```

Run it:

```bash
./hello.sh
```

Output:

```text
Hello
```

---

# Variables

Create variables:

```bash
name="Usman"

echo $name
```

Output:

```text
Usman
```

---

# User Input

```bash
read -p "Enter your name: " name

echo "Hello $name"
```

---

# Comments

Single-line comment:

```bash
# This is a comment
```

Comments improve script readability.

---

# Conditional Statements

Example:

```bash
if [ -f test.txt ]; then
    echo "File exists"
else
    echo "File not found"
fi
```

---

# Comparison Operators

| Operator | Meaning |
|----------|----------|
| `-eq` | Equal |
| `-ne` | Not Equal |
| `-gt` | Greater Than |
| `-lt` | Less Than |
| `-ge` | Greater Than or Equal |
| `-le` | Less Than or Equal |

Example:

```bash
if [ 5 -gt 3 ]; then
    echo "True"
fi
```

---

# Loops

## For Loop

```bash
for i in 1 2 3
do
    echo $i
done
```

Output:

```text
1
2
3
```

---

## While Loop

```bash
count=1

while [ $count -le 5 ]
do
    echo $count
    count=$((count + 1))
done
```

---

# Functions

```bash
hello() {
    echo "Hello Android"
}

hello
```

---

# Command Substitution

Store command output in a variable:

```bash
current_user=$(whoami)

echo $current_user
```

---

# Arguments

Script:

```bash
#!/bin/bash

echo $1
```

Run:

```bash
./test.sh Android
```

Output:

```text
Android
```

`$1` is the first argument.

---

# Exit Codes

Every command returns an exit status.

```text
0 = Success

Non-zero = Error
```

Check the previous command's status:

```bash
echo $?
```

---

# Common Bash Operators

| Operator | Meaning |
|----------|----------|
| `&&` | Run next command if previous succeeds |
| `||` | Run next command if previous fails |
| `;` | Run commands sequentially |
| `|` | Pipe output to another command |

Example:

```bash
mkdir test && cd test
```

---

# Useful Bash Commands

Print text:

```bash
echo "Hello"
```

Current directory:

```bash
pwd
```

List files:

```bash
ls
```

Current user:

```bash
whoami
```

Find files:

```bash
find . -name "*.apk"
```

Search text:

```bash
grep password config.txt
```

---

# Android Automation with Bash

Example: List installed packages

```bash
#!/bin/bash

adb shell pm list packages
```

---

# Pull an APK Automatically

```bash
#!/bin/bash

PACKAGE="com.example.app"

adb shell pm path $PACKAGE
```

---

# Install an APK

```bash
#!/bin/bash

adb install app.apk
```

---

# Capture Logcat

```bash
#!/bin/bash

adb logcat > logs.txt
```

---

# Take a Screenshot

```bash
#!/bin/bash

adb shell screencap /sdcard/screen.png
adb pull /sdcard/screen.png
```

---

# Collect Application Data

```bash
#!/bin/bash

PACKAGE=com.example.app

adb shell run-as $PACKAGE ls
```

---

# Multiple Devices

List devices:

```bash
adb devices
```

Example loop:

```bash
for device in $(adb devices | awk 'NR>1 && $2=="device"{print $1}')
do
    echo $device
done
```

---

# Bash Scripting from a Pentester's Perspective

Bash scripts are commonly used to automate:

- APK extraction
- APK installation
- ADB commands
- Frida startup
- Log collection
- File collection
- Database extraction
- Report generation
- Bulk testing of multiple APKs

---

# Example Android Pentesting Workflow

```text
Start Script
      ↓
Check Connected Device
      ↓
Install APK
      ↓
Launch Application
      ↓
Capture Logs
      ↓
Pull APK
      ↓
Extract Files
      ↓
Save Results
```

---

# Common Automation Tasks

| Task | Bash Command |
|------|--------------|
| Check connected devices | `adb devices` |
| Install APK | `adb install app.apk` |
| Uninstall app | `adb uninstall <package>` |
| Start app | `adb shell monkey -p <package> 1` |
| View logs | `adb logcat` |
| Pull files | `adb pull` |
| Push files | `adb push` |
| Capture screenshot | `adb shell screencap` |
| Record screen | `adb shell screenrecord` |

---

# Example: APK Backup Script

```bash
#!/bin/bash

PACKAGE="com.example.app"

APK_PATH=$(adb shell pm path $PACKAGE | cut -d: -f2)

adb pull "$APK_PATH"
```

This script:

1. Gets the APK path.
2. Extracts the path.
3. Pulls the APK to your computer.

---

# Best Practices

- Add comments to explain complex logic.
- Use meaningful variable names.
- Check command exit codes.
- Validate user input.
- Quote variables (e.g., `"$file"`) to handle spaces safely.
- Test scripts on a non-production environment before using them on important devices.

---

# Key Takeaways

- **Bash Scripting** automates Linux shell commands by storing them in executable script files.
- Every Bash script typically begins with a **Shebang (`#!/bin/bash`)**.
- Bash supports variables, loops, conditions, functions, and command-line arguments, making it powerful for automation.
- Android penetration testers use Bash scripts to automate repetitive tasks such as **ADB operations**, **APK extraction**, **log collection**, and **device management**.
- Learning Bash scripting improves efficiency, reduces manual effort, and helps build custom tools for Android security testing.
