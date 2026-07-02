# Linux Package Management - Android Pentesting

## What is Package Management?

**Package Management** is the process of **installing, updating, upgrading, configuring, and removing software** on a Linux system.

Instead of downloading software manually, Linux uses **package managers** to automate software management and dependency handling.

Example:

```text
User
   │
   ▼
Package Manager
   │
   ▼
Downloads Package
   │
   ▼
Installs Dependencies
   │
   ▼
Installs Software
```

---

# Why is Package Management Important?

Package management helps you:

- Install software easily
- Update software securely
- Remove unused applications
- Manage dependencies
- Keep the system secure
- Install pentesting tools quickly

For Android Pentesting, you'll frequently install tools such as:

- ADB
- Fastboot
- Frida
- JADX
- apktool
- Ghidra
- MobSF
- Burp Suite (manual download)
- Objection

---

# What is a Package?

A **package** is a file that contains:

- Application files
- Libraries
- Configuration files
- Metadata
- Installation instructions

Examples:

Ubuntu:

```text
.deb
```

Fedora:

```text
.rpm
```

Android:

```text
.apk
```

---

# Package Manager vs Package

| Package | Package Manager |
|----------|-----------------|
| Software bundle | Tool used to install and manage packages |
| Example: `.deb` | Example: `apt` |
| Contains application files | Downloads, installs, updates, and removes packages |

---

# Popular Linux Package Managers

| Distribution | Package Manager | Package Format |
|--------------|-----------------|----------------|
| Debian | `apt` | `.deb` |
| Ubuntu | `apt` | `.deb` |
| Kali Linux | `apt` | `.deb` |
| Fedora | `dnf` | `.rpm` |
| CentOS | `yum` / `dnf` | `.rpm` |
| Arch Linux | `pacman` | `.pkg.tar.zst` |

Since Kali Linux is based on Debian, you'll primarily use **APT**.

---

# APT (Advanced Package Tool)

APT is the default package manager on:

- Debian
- Ubuntu
- Kali Linux

---

# Update Package List

Refresh the package index:

```bash
sudo apt update
```

This updates the list of available packages but **does not install updates**.

---

# Upgrade Installed Packages

Upgrade installed software:

```bash
sudo apt upgrade
```

---

# Full Upgrade

Upgrade packages and allow dependency changes:

```bash
sudo apt full-upgrade
```

---

# Install a Package

Example:

```bash
sudo apt install nmap
```

Install multiple packages:

```bash
sudo apt install adb fastboot
```

---

# Remove a Package

Remove a package while keeping configuration files:

```bash
sudo apt remove nmap
```

---

# Purge a Package

Remove a package and its configuration files:

```bash
sudo apt purge nmap
```

---

# Remove Unused Dependencies

```bash
sudo apt autoremove
```

---

# Search for a Package

```bash
apt search jadx
```

---

# Show Package Information

```bash
apt show nmap
```

Example output:

```text
Version
Description
Dependencies
Homepage
```

---

# List Installed Packages

```bash
apt list --installed
```

---

# Check Package Version

```bash
apt policy adb
```

---

# Install Local Package

Example:

```bash
sudo dpkg -i package.deb
```

If dependencies are missing:

```bash
sudo apt --fix-broken install
```

---

# Download a Package Without Installing

```bash
apt download nmap
```

---

# Package Repositories

APT downloads packages from online repositories.

Example:

```text
User
   │
   ▼
APT
   │
   ▼
Repository
   │
   ▼
Download Package
   │
   ▼
Install
```

Repository configuration:

```text
/etc/apt/sources.list
```

---

# Common Package Management Commands

Update package list:

```bash
sudo apt update
```

Upgrade packages:

```bash
sudo apt upgrade
```

Install:

```bash
sudo apt install package
```

Remove:

```bash
sudo apt remove package
```

Search:

```bash
apt search package
```

Show package info:

```bash
apt show package
```

---

# Installing Android Pentesting Tools

Install ADB and Fastboot:

```bash
sudo apt install adb fastboot
```

Install JADX (if available):

```bash
sudo apt install jadx
```

Install apktool:

```bash
sudo apt install apktool
```

Install Nmap:

```bash
sudo apt install nmap
```

Install tcpdump:

```bash
sudo apt install tcpdump
```

Install OpenSSL:

```bash
sudo apt install openssl
```

---

# Verify Installation

Check tool versions:

ADB:

```bash
adb version
```

Fastboot:

```bash
fastboot --version
```

apktool:

```bash
apktool --version
```

JADX:

```bash
jadx --version
```

---

# Android Package Management

Linux package management installs software on your computer.

Android has its own package manager called **Package Manager (PM)**, which manages Android applications (`.apk`).

ADB provides access to the `pm` command.

List installed apps:

```bash
adb shell pm list packages
```

Install an APK:

```bash
adb install app.apk
```

Uninstall an app:

```bash
adb uninstall com.example.app
```

Get APK path:

```bash
adb shell pm path com.example.app
```

---

# Linux Package Manager vs Android Package Manager

| Linux | Android |
|--------|----------|
| Installs Linux software | Installs Android applications |
| Uses `apt`, `dnf`, `pacman` | Uses `pm` |
| Package format: `.deb`, `.rpm` | Package format: `.apk`, `.aab` (distributed via Play, converted to APKs on device) |

---

# Package Management from a Pentester's Perspective

Package management helps you:

- Install pentesting tools
- Keep tools updated
- Manage dependencies
- Install Android SDK components
- Set up testing environments

Android Package Manager helps you:

- Enumerate installed apps
- Extract APK paths
- Install test applications
- Remove applications
- Inspect package information

---

# Example Android Pentesting Workflow

```text
Install Tools
      │
      ▼
ADB
      │
      ▼
Connect Device
      │
      ▼
Install APK
      │
      ▼
Analyze Application
```

---

# Common Commands

Linux:

```bash
sudo apt update
sudo apt install adb
sudo apt install apktool
sudo apt install nmap
```

Android:

```bash
adb install app.apk
adb uninstall com.example.app
adb shell pm list packages
adb shell pm path com.example.app
```

---

# Best Practices

- Run `sudo apt update` before installing new packages.
- Keep your tools updated with `sudo apt upgrade`.
- Install software from trusted repositories or official sources.
- Remove unused packages using `sudo apt autoremove`.
- Verify installed tool versions after installation.

---

# Key Takeaways

- **Package Management** is the process of installing, updating, upgrading, and removing software.
- A **package manager** automates software installation and dependency management.
- Kali Linux primarily uses **APT (`apt`)** to manage `.deb` packages.
- Android uses a separate **Package Manager (`pm`)** to manage application packages (`.apk`).
- Android penetration testers rely on Linux package managers to install tools and on Android's Package Manager to install, remove, and inspect mobile applications.
