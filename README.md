# Airmark

Airmark is a macOS 13+ menu-bar app that puts a watermark on one selected display without blocking the keyboard, mouse, or menu bar.

<p>
  <a href="https://github.com/anujraja404/airmark/releases/latest">
    <strong>Download the latest DMG</strong>
  </a>
</p>

## How It Works

```mermaid
flowchart TB
  Tray["Menu bar tray"] --> Menu["Enable / Settings / Display / Quit"]
  Menu --> Settings["Native macOS settings window"]
  Settings --> State["Persisted settings"]
  State --> Overlay["Transparent click-through overlay"]
  State --> Renderer["React watermark renderer"]
  Renderer --> Overlay
  Overlay --> Display["Selected display only"]
```

## Features

- Dockless macOS utility.
- Tray menu for enable/disable, settings, display selection, and quit.
- Text mode with opacity, size, and spacing controls.
- Image mode with drag/drop, file picker, and clipboard paste.
- Settings persist across relaunches.
- Launch-at-login support.

## Screenshots

![Tray menu](docs/screenshots/tray-menu.png)
![Text mode settings](docs/screenshots/settings-text.png)
![Image mode settings](docs/screenshots/settings-image.png)

## Install

1. Open the DMG.
2. Drag `Airmark.app` into `Applications`.
3. Launch Airmark from `Applications` or the menu bar.

## Develop

```bash
npm install
npm run tauri dev
```

## Build

```bash
npm run tauri build
```

Release artifacts:

- `src-tauri/target/release/bundle/macos/Airmark.app`
- `src-tauri/target/release/bundle/dmg/Airmark_0.1.0_aarch64.dmg`
