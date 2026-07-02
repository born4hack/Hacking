# Window Manager

## Definition

The **Window Manager** is an Android system service responsible for managing **windows** displayed on the screen.

It controls:

- Creating windows
- Displaying windows
- Moving windows
- Resizing windows
- Window animations
- Window focus
- Screen rotation
- Window layering (which window appears on top)

The actual system service is called the **Window Manager Service (WMS)** and runs inside the **System Server** process.

Example:

```text
Application

↓

Window Manager

↓

Display Window

↓

User Sees Screen
```

Without the Window Manager, Android applications would not be able to display their user interfaces.

---

# What is a Window?

A **Window** is the area of the screen where an application displays its user interface.

Example:

```text
+-----------------------------+
|        Chrome Window        |
|                             |
|   Google Search Page        |
|                             |
+-----------------------------+
```

Every Activity is displayed inside a Window.

---

# Why Does Android Need a Window Manager?

Imagine several applications running at the same time.

```text
Chrome

↓

WhatsApp

↓

Camera

↓

Keyboard
```

Android must decide:

- Which window is visible
- Which window is hidden
- Which window receives touch events
- Which window is on top

The Window Manager handles these responsibilities.

---

# Where Does Window Manager Fit?

The Window Manager is a framework service running inside the System Server.

```text
Application

↓

Binder IPC

↓

Window Manager Service (WMS)

↓

SurfaceFlinger

↓

Display
```

Notice that **SurfaceFlinger** is responsible for actually composing and rendering frames to the display, while WMS manages the windows themselves.

---

# Window Manager vs Window Manager Service (WMS)

These terms are related but different.

## Window Manager

The API that applications use.

Example:

```text
Application

↓

WindowManager API
```

Developers use this API to create and manage windows.

---

## Window Manager Service (WMS)

The actual system service.

Example:

```text
Application

↓

Binder

↓

Window Manager Service
```

WMS performs all window management operations.

---

# Responsibilities of Window Manager

The Window Manager performs many important tasks.

```text
Window Manager

├── Create Windows
├── Remove Windows
├── Resize Windows
├── Move Windows
├── Window Focus
├── Screen Rotation
├── Window Animation
├── Input Routing
└── Window Layering
```

---

# Step 1 — Create a Window

When an Activity starts:

```text
Activity

↓

Request Window

↓

Window Manager

↓

Create Window
```

The Window Manager allocates a new window for the Activity.

---

# Step 2 — Display the Window

After creation:

```text
Window Manager

↓

SurfaceFlinger

↓

GPU

↓

Display
```

The user now sees the application's interface.

---

# Step 3 — Window Gets Focus

Only one window receives user input at a time.

Example:

```text
Chrome

↓

Focused Window

↓

Receives Keyboard

↓

Receives Touch
```

Other windows remain inactive until they gain focus.

---

# Window Layering (Z-Order)

Android stacks windows in layers.

Example:

```text
Top

Popup Window

↓

Dialog

↓

Application Window

↓

Wallpaper

Bottom
```

The Window Manager determines which window appears above another.

---

# Example — Incoming Phone Call

Suppose the user is browsing Chrome.

```text
Chrome Window
```

Now an incoming call arrives.

```text
Incoming Call Window

↓

Appears Above

↓

Chrome Window
```

The call interface receives focus because it has a higher priority.

---

# Window Types

Android supports multiple window types.

Examples include:

- Application Window
- Dialog Window
- Popup Window
- System Window
- Toast Window
- Input Method Window (Keyboard)

Example:

```text
Application

↓

Create Dialog

↓

Window Manager

↓

Dialog Window
```

---

# Managing Screen Rotation

When the device rotates:

```text
Portrait

↓

Rotate Device

↓

Landscape
```

The Window Manager recalculates:

- Window size
- Position
- Orientation
- Layout

This ensures the interface fits the new screen orientation.

---

# Window Resizing

Suppose the device enters split-screen mode.

```text
Chrome

↓

Split Screen

↓

Resize Window

↓

50% Screen
```

WMS adjusts the window dimensions accordingly.

---

# Window Animation

The Window Manager controls transition animations.

Example:

```text
Open App

↓

Fade Animation

↓

Window Appears
```

Examples include:

- Fade
- Slide
- Zoom
- Enter
- Exit

---

# Input Routing

Touch and keyboard events must be delivered to the correct window.

Example:

```text
User Touch

↓

Window Manager

↓

Focused Window

↓

Application
```

Only the active window receives input.

---

# Multi-Window Support

Modern Android supports multiple application windows.

Example:

```text
+-------------+-------------+
|   Chrome    | WhatsApp    |
|             |             |
+-------------+-------------+
```

The Window Manager coordinates the layout and interaction between multiple windows.

---

# Window Manager and SurfaceFlinger

A common misconception is that the Window Manager draws the screen.

Actually:

```text
Window Manager

↓

Manage Windows

↓

SurfaceFlinger

↓

Combine Surfaces

↓

Display
```

Responsibilities:

**Window Manager**

- Window creation
- Focus management
- Layout
- Rotation
- Z-order
- Input routing

**SurfaceFlinger**

- Composites graphics
- Combines window surfaces
- Sends final frames to the display

---

# Window Manager and Activity Manager

These two services work together.

```text
User Opens App

↓

Activity Manager

↓

Start Activity

↓

Window Manager

↓

Create Window

↓

Display Screen
```

AMS manages the application lifecycle.

WMS manages how the application's window appears.

---

# Window Manager and Security

The Window Manager enforces several security rules.

Examples include:

- Prevent unauthorized overlays
- Secure window flags
- Lock screen behavior
- Input focus protection
- Touch event routing

For example:

```text
Sensitive Banking Screen

↓

FLAG_SECURE

↓

Prevent Screenshots
```

Applications can use secure window flags to protect sensitive content.

---

# Window Overlays

Some applications display windows above other apps.

Example:

```text
Facebook Messenger

↓

Chat Head

↓

Floating Window
```

This is implemented using special overlay windows.

Android restricts these overlays because they can be abused.

---

# Window Manager from a Pentester's Perspective

The Window Manager is related to several Android security topics.

Common attack areas include:

- Tapjacking
- Overlay attacks
- Clickjacking
- Screen capture bypass
- Secure window flag misuse
- Lock screen bypass
- Window injection
- UI spoofing

Many phishing attacks on Android rely on malicious overlay windows.

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

Start Activity

↓

Window Manager

↓

Create Window

↓

SurfaceFlinger

↓

Display Login Screen
```

Now consider a malicious application.

```text
Malicious App

↓

Overlay Window

↓

Displayed Above Banking App

↓

User Enters Password

↓

Credentials Stolen
```

Modern Android versions place restrictions on overlay windows and require special permissions (such as `SYSTEM_ALERT_WINDOW`) to reduce the risk of these attacks.

---

# Key Terms

| Term | Meaning |
|------|---------|
| Window | Area of the screen where an Activity is displayed |
| Window Manager | API used to create and manage windows |
| Window Manager Service (WMS) | System service responsible for window management |
| SurfaceFlinger | System compositor that combines surfaces and renders frames to the display |
| Focus | Window currently receiving user input |
| Z-Order | Order in which windows are stacked on the screen |
| Overlay Window | Window displayed above other applications |
| Dialog | Small window displayed over another window |
| Split Screen | Mode where multiple application windows are displayed simultaneously |
| FLAG_SECURE | Window flag that helps prevent screenshots and screen recording |

---

# Key Takeaways

- The Window Manager Service (WMS) manages all windows displayed on an Android device.
- Every Activity is displayed inside a Window managed by WMS.
- WMS is responsible for window creation, focus, layout, rotation, resizing, animations, and input routing.
- SurfaceFlinger handles rendering and compositing, while WMS manages window behavior and organization.
- WMS works closely with the Activity Manager to display application user interfaces.
- Window management plays a critical role in Android security by controlling overlays, secure windows, and user input.
- Understanding the Window Manager is essential for learning Android UI architecture and security topics such as Tapjacking, overlay attacks, and UI spoofing.
