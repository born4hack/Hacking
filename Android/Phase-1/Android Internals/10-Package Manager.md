# Package Manager

## Definition

The **Package Manager** is an Android system service responsible for managing everything related to Android applications (packages).

It handles:

- Installing applications
- Uninstalling applications
- Updating applications
- Verifying APKs
- Managing permissions
- Maintaining information about installed apps

In Android, every application is called a **package**, which is why the service is called the **Package Manager**.

Example:

```text
Install APK

↓

Package Manager

↓

Verify APK

↓

Install App

↓

Register Package
```

Without the Package Manager, Android would not be able to install or manage applications.

---

# Why is it Called "Package" Manager?

Android applications are distributed as **APK (Android Package)** files.

Example:

```text
WhatsApp.apk

Instagram.apk

Chrome.apk
```

Each APK is considered a package because it contains everything required to run an application.

Example:

```text
APK

├── Code
├── Resources
├── Images
├── Native Libraries
├── Assets
└── AndroidManifest.xml
```

The Package Manager manages these packages.

---

# Where Does Package Manager Fit?

The Package Manager is one of the core services running inside the **System Server** process.

```text
Applications
      │
      ▼
Binder IPC
      │
      ▼
System Server
      │
      ▼
Package Manager Service (PMS)
```

Applications communicate with the Package Manager using **Binder IPC**.

---

# Package Manager vs Package Manager Service (PMS)

There are two related terms that are often confused.

## Package Manager

The API used by applications.

Example:

```text
Application

↓

PackageManager API
```

Apps use this API to query information about installed packages.

---

## Package Manager Service (PMS)

The actual system service running inside the System Server.

Example:

```text
Application

↓

Binder

↓

Package Manager Service
```

PMS performs the real work.

---

# Responsibilities of Package Manager

The Package Manager performs many important tasks.

```text
Package Manager

├── Install APK
├── Uninstall APK
├── Update APK
├── Verify APK
├── Parse Manifest
├── Assign UID
├── Grant Permissions
├── Maintain Package Database
└── Query Installed Apps
```

---

# Step 1 — Installing an APK

When a user installs an APK:

```text
User

↓

Tap Install

↓

Package Manager

↓

Verify APK

↓

Install Application
```

The Package Manager performs several checks before installation.

---

# APK Installation Process

```text
APK File

↓

Parse AndroidManifest.xml

↓

Verify Signature

↓

Check Package Name

↓

Assign UID

↓

Copy Files

↓

Register Application

↓

Installation Complete
```

---

# Step 2 — Parse AndroidManifest.xml

Every Android application contains an **AndroidManifest.xml** file.

The Package Manager reads it during installation.

Example:

```text
AndroidManifest.xml

↓

Package Name

↓

Permissions

↓

Activities

↓

Services

↓

Broadcast Receivers

↓

Content Providers
```

This information tells Android how the application should behave.

---

# Step 3 — Verify APK Signature

Every APK is digitally signed.

The Package Manager verifies this signature.

Example:

```text
APK

↓

Read Certificate

↓

Verify Signature

↓

Trusted?
```

If verification fails:

```text
Installation

↓

Rejected
```

This helps ensure the APK has not been modified after signing.

---

# Step 4 — Check Package Name

Every application has a unique package name.

Example:

```text
com.whatsapp

com.instagram.android

com.android.settings
```

Android uses the package name as the application's unique identifier.

If a conflicting package is detected, installation may fail unless it is a valid update signed with the same key.

---

# Step 5 — Assign Linux UID

Every installed application receives a unique Linux User ID (UID).

Example:

```text
Chrome

↓

UID 10120
```

```text
WhatsApp

↓

UID 10145
```

This UID is used for Android's sandboxing.

Different applications usually have different UIDs.

---

# Step 6 — Grant Permissions

The Package Manager processes the permissions requested by the application.

Example:

```text
AndroidManifest.xml

↓

Camera Permission

↓

Location Permission

↓

Internet Permission
```

Depending on the permission type and Android version:

- Automatically granted
- Granted during installation
- Requested at runtime

---

# Step 7 — Copy Application Files

The APK is copied to the appropriate storage location.

Example:

```text
APK

↓

/data/app/

↓

Installed
```

Android also prepares application data directories.

---

# Step 8 — Register the Application

Finally, the application is added to Android's package database.

Example:

```text
Package Database

↓

New Package Registered

↓

Launcher Shows App Icon
```

Now the application can be launched.

---

# Uninstalling an App

When uninstalling:

```text
User

↓

Uninstall

↓

Package Manager

↓

Remove APK

↓

Delete App Data

↓

Remove Package
```

The Package Manager removes:

- APK
- Data
- Cache
- Package information

unless the user chooses to retain app data (where supported).

---

# Updating an App

Updating is similar to installation.

```text
New APK

↓

Package Manager

↓

Verify Signature

↓

Replace Old Version

↓

Update Complete
```

The update must usually be signed with the same certificate as the installed application.

---

# Querying Installed Applications

Applications can request information about installed packages.

Example:

```text
Application

↓

Package Manager

↓

Installed Packages

↓

Return Results
```

Modern Android versions restrict this capability to protect user privacy.

---

# Package Information

The Package Manager stores information such as:

- Package Name
- Version
- UID
- Permissions
- Activities
- Services
- Broadcast Receivers
- Content Providers
- Signing Certificate

Example:

```text
Package

├── Name
├── Version
├── UID
├── Permissions
└── Components
```

---

# Package Manager Database

The Package Manager maintains a database of installed applications.

Example:

```text
Installed Packages

├── Chrome

├── Gmail

├── WhatsApp

├── Instagram

└── Banking App
```

Android consults this information whenever an app is launched or queried.

---

# Package Manager and Binder

Applications communicate with the Package Manager through Binder IPC.

Example:

```text
Application

↓

Binder

↓

Package Manager

↓

Response
```

Applications never access PMS directly.

---

# Package Manager and Security

The Package Manager enforces several important security mechanisms.

Examples include:

- APK signature verification
- Permission management
- UID assignment
- Package name uniqueness
- Shared UID validation (legacy feature)
- Package visibility restrictions

Example:

```text
APK

↓

Signature Check

↓

Trusted?

↓

Install
```

---

# Package Manager from a Pentester's Perspective

The Package Manager plays a critical role in Android security.

Common security topics include:

- APK signature verification
- Package name spoofing
- Shared UID abuse (legacy)
- Permission analysis
- Exported component discovery
- Package visibility
- Fake application attacks
- Application enumeration

Understanding how PMS works helps explain why applications receive unique UIDs and how Android enforces application isolation.

---

# Real-World Example

Suppose a user installs a banking application.

```text
User

↓

Download APK

↓

Package Manager

↓

Read Manifest

↓

Verify Signature

↓

Assign UID

↓

Grant Permissions

↓

Install App
```

Now consider an attacker attempting to install a fake update.

```text
Fake APK

↓

Package Manager

↓

Verify Signature

↓

Signature Mismatch

↓

Installation Rejected
```

The Package Manager prevents the fake application from replacing the legitimate one because it is not signed with the same certificate.

---

# Key Terms

| Term | Meaning |
|------|---------|
| Package | An Android application |
| APK | Android Package containing an application |
| Package Manager | API used to manage installed applications |
| Package Manager Service (PMS) | System service responsible for package management |
| AndroidManifest.xml | File describing an application's components and permissions |
| Package Name | Unique identifier for an Android application |
| UID | Linux User ID assigned to each application |
| Binder IPC | Communication mechanism used to access PMS |
| Signature | Digital certificate used to verify application integrity |
| Runtime Permission | Permission requested while the application is running |

---

# Key Takeaways

- The Package Manager is responsible for installing, updating, uninstalling, and managing Android applications.
- Every Android application is packaged as an APK containing code, resources, and an AndroidManifest.xml file.
- The Package Manager Service (PMS) runs inside the System Server and is accessed through Binder IPC.
- During installation, PMS parses the manifest, verifies the APK signature, assigns a unique Linux UID, grants permissions, and registers the application.
- Every application receives its own UID, which is a key part of Android's sandboxing model.
- APK signature verification prevents unauthorized applications from replacing legitimate ones.
- Understanding the Package Manager is essential for Android internals, application security, reverse engineering, and Android penetration testing.
