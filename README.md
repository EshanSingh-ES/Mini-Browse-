# 🧭 MiniBrowse

**MiniBrowse** is a minimalist floating web browser for macOS. It enables always-on-top web access—ideal for referencing, light research, or multi-tasking across apps.

[![Watch the Demo on YouTube](https://img.youtube.com/vi/OFLy_V1gogI/hqdefault.jpg)](https://youtu.be/OFLy_V1gogI)
---

## 🧱 Architecture Overview

MiniBrowse is structured around custom SwiftUI views, native macOS window management (via `NSPanel`), and utility classes for clipboard, download, and float behavior.

Here’s a breakdown of each core component:

---

## 📂 Source File Explanations

### ✅ `AppDelegate.swift`

The **main entry point** of the app.

- Configures the application lifecycle.
- Instantiates the **Search Bar Window** on launch.
- Hooks up clipboard monitoring via `ClipBoardManager`.
- Registers global services like `FloatManager`.

📌 *Think of this as the conductor that kicks off everything when the app launches.*

---

### 🔍 `SearchBarWindow.swift`

A **custom floating search bar window** built with `NSPanel`.

- Accepts URLs or search queries.
- When submitted, it triggers `FloatManager` to open a mini browser window.
- Floats independently from main browser windows.
- Lightweight, persistent, and unobtrusive.

📌 *Acts like a spotlight search—but for the web.*

---

### 🌐 `MainWebBrowserView.swift`

The **main SwiftUI view** that wraps `WKWebView`.

- Hosts a toolbar with back, forward, refresh, and download buttons.
- Displays web content in a resizable, draggable floating panel.
- Connects to `DownloadManager` for downloading web resources.
- Custom navigation handling and progress indication.

📌 *This is your in-app mini browser.*

---

### 🪟 `NSWindowCoordinator.swift`

Handles **native window styling and behavior** using `NSWindow`.

- Converts windows into `NSPanel` (a special always-on-top macOS window).
- Removes title bars, sets window level to float above normal windows.
- Configures transparency, click-through behavior, and rounded corners.

📌 *Makes the browser window float and behave like a utility panel.*

---

### 🧭 `FloatManager.swift`

A singleton responsible for **creating and managing floating web windows**.

- Provides a function `createWindow(for url: URL)` to launch new floating browsers.
- Coordinates the geometry, window style, and initialization logic.
- Internally uses `FloatManagerCoordinator`.

📌 *The window spawner.*

---

### 🤝 `FloatManagerCoordinator.swift`

Acts as a **controller between the UI and macOS windowing system**.

- Provides helpers for laying out and managing multiple floating windows.
- Ensures each new web browser window behaves consistently (size, position, styling).
- Bridges SwiftUI views with native NSWindow logic.

📌 *The bridge between SwiftUI and AppKit.*

---

### 📋 `ClipBoardManager.swift`

Watches the macOS clipboard and **detects when URLs are copied**.

- Polls the clipboard periodically for changes.
- Parses and validates copied content (e.g., URLs).
- Automatically sends the copied link to the `SearchBarWindow`.

📌 *Copy a link, and it’s ready to paste—automatically.*

---


### 💾 `DownloadManager.swift`

Manages **web downloads** initiated from inside `WKWebView`.

- Uses `WKDownloadDelegate` to handle files downloaded via the browser.
- Saves files to the user’s Downloads folder.
- Alerts or logs download progress/errors.

📌 *Allows MiniBrowse to handle file downloads like a real browser.*

---

## 🛠 Example Flow

1. **Launch App**: `AppDelegate` opens the floating `SearchBarWindow`.
2. **Clipboard URL**: `ClipBoardManager` detects a copied link.
3. **User hits Enter**: `SearchBarWindow` sends the URL to `FloatManager`.
4. **Window opens**: `FloatManagerCoordinator` + `NSWindowCoordinator` spawn a floating `MainWebBrowserView`.
5. **Browsing starts**: User can navigate, download, or open more windows.

---

