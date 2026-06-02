# Airmark

Airmark is a macOS 13+ menu-bar watermark utility built with Tauri 2, Rust, and React. It runs without a Dock icon, keeps the overlay click-through, and lets you target a single display or choose a display per session from the tray menu.

## What It Does

- Menu-bar app with `Enable/Disable`, `Open Settings`, `Choose Display`, and `Quit`.
- Persistent settings window with native macOS chrome.
- Text watermark mode with opacity, text size, and text spacing sliders.
- Image watermark mode with drag-and-drop, file chooser, and clipboard paste support.
- Overlay stays off the menu bar and is bound to the selected display work area.
- Settings persist across relaunches.
- Launch-at-login support.

## Install

Open the DMG and drag `Airmark.app` into `Applications`:

`src-tauri/target/release/bundle/dmg/Airmark_0.1.0_aarch64.dmg`

## Run In Dev

```bash
npm install
npm run tauri dev
```

## Build

```bash
npm run tauri build
```

Release artifacts are written to:

- `src-tauri/target/release/bundle/macos/Airmark.app`
- `src-tauri/target/release/bundle/dmg/Airmark_0.1.0_aarch64.dmg`

## Sign And Notarize

Set environment variables:

```bash
export APP_PATH="/Users/macbookpro/Developer/airmark/src-tauri/target/release/bundle/macos/Airmark.app"
export APPLE_DEV_ID="Developer ID Application: YOUR_NAME (TEAMID)"
export APPLE_ID="you@example.com"
export TEAM_ID="TEAMID"
export APP_SPECIFIC_PASSWORD="xxxx-xxxx-xxxx-xxxx"
```

Sign:

```bash
codesign --force --deep --options runtime --timestamp \
  --sign "$APPLE_DEV_ID" "$APP_PATH"
codesign --verify --deep --strict --verbose=2 "$APP_PATH"
```

Package for notarization:

```bash
ditto -c -k --keepParent "$APP_PATH" /tmp/Airmark.zip
```

Submit and wait:

```bash
xcrun notarytool submit /tmp/Airmark.zip \
  --apple-id "$APPLE_ID" \
  --team-id "$TEAM_ID" \
  --password "$APP_SPECIFIC_PASSWORD" \
  --wait
```

Staple and verify:

```bash
xcrun stapler staple "$APP_PATH"
spctl --assess --type execute --verbose "$APP_PATH"
```

## Acceptance Checklist

- [ ] App launches without a Dock icon.
- [ ] Overlay stays disabled until the user confirms setup.
- [ ] Tray menu exposes enable/disable, settings, display selection, and quit.
- [ ] Display selection updates the overlay target immediately.
- [ ] Text mode and image mode both render correctly.
- [ ] Clipboard image paste accepts copied image data from apps like ChatGPT or Preview.
- [ ] Settings persist after quit and relaunch.
- [ ] DMG opens with the standard drag-to-Applications flow.

## Architecture Notes

- Rust owns tray, overlay, display, and app-state logic.
- Frontend owns settings UI and overlay rendering.
- macOS-specific AppKit behavior is isolated in `src-tauri/src/macos_shim.rs`.

