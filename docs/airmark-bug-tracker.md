# Airmark Bug Tracker

Last updated: 2026-06-01

## Fixed

- **AM-001: Overlay blocks the desktop and hides apps**
  - Symptom: A full-screen pale overlay appeared and made the MacBook screen difficult or impossible to use.
  - Cause: The overlay webview inherited an opaque page background, was focusable, and was explicitly focused after showing.
  - Fix: Overlay is now non-focusable, never receives focus, uses transparent `html/body/#root` backgrounds, and defaults to disabled until confirmed.

- **AM-002: App enables overlay immediately on first launch**
  - Symptom: Launching the app could immediately cover the selected display.
  - Cause: Default settings had `enabled: true`.
  - Fix: New installs and migrated settings start with `enabled: false` until setup is confirmed.

- **AM-003: Watermark covers macOS menu bar area**
  - Symptom: Overlay could occupy the full physical display including menu-bar space.
  - Cause: Overlay bounds used monitor size and position.
  - Fix: Overlay now uses monitor `work_area`, keeping the menu bar/system-reserved area clear.

- **AM-004: First-run display selection is not explicit**
  - Symptom: User did not get a clear screen-selection/confirmation step before enabling.
  - Fix: First launch opens the settings window, preselects the primary display, and requires `Confirm & Enable`.

- **AM-005: Settings UI feels oversized and non-native**
  - Symptom: The old settings window looked like a web form rather than a professional macOS utility.
  - Fix: Replaced normal settings flow with a compact menu-bar controls popover using native-style material, switches, segmented mode control, sliders, compact typography, and mode-specific panels.

- **AM-006: Popover appears on wrong display/desktop**
  - Symptom: Clicking the menu-bar icon could show controls on the left-side desktop instead of under the icon.
  - Fix: Popover positioning now uses the tray icon rectangle, centers below it, and clamps to the same monitor's usable bounds.

- **AM-007: Display names are hardware IDs**
  - Symptom: Selector showed values like `Monitor #23497`.
  - Fix: macOS display names now prefer `NSScreen.localizedName`, with `Primary Display` / `Display N` fallback labels.

- **AM-008: Popover remains too window-like**
  - Symptom: Controls felt closer to a small settings window than a menu-bar product popover.
  - Fix: Controls window is smaller, has a popover pointer, hides on focus loss/outside click, and uses tighter native-style rows.

- **AM-009: Image import needs one-step paste**
  - Symptom: Users had to drag or manually choose an image even when they had just copied the file.
  - Fix: Image mode now includes a Paste button that accepts a copied image file URL from the macOS pasteboard.

- **AM-010: Persistent image setup window**
  - Symptom: Auto-closing menu popover disappeared when users clicked outside it, preventing drag/drop and making image setup fragile.
  - Fix: Settings now opens as a persistent compact window and only closes from its explicit close control.

- **AM-011: Pasted images from ChatGPT/browser**
  - Symptom: Paste only accepted copied Finder image files, not copied image pixels from ChatGPT or browsers.
  - Fix: macOS pasteboard support now accepts PNG clipboard data and TIFF clipboard data converted to PNG, saves it locally, and applies it as the image watermark.

## Open QA Items

- **AM-QA-001: Multi-monitor behavior**
  - Verify on a real multi-monitor setup that selecting each display moves the overlay to that display's work area only.

- **AM-QA-002: Click-through behavior across apps**
  - Verify clicks, typing, window dragging, Mission Control, and menu-bar access continue to work while the overlay is enabled.

- **AM-QA-003: Tray menu visibility**
  - Verify the menu-bar icon opens the tray menu on left click and right click on all attached displays.

- **AM-QA-004: Tray popover anchoring**
  - Verify the controls popover opens directly below the Airmark menu-bar icon on the active display, including external-monitor menu bars.

- **AM-QA-005: Popover dismissal**
  - Verify clicking outside the controls popover closes it while controls remain usable inside the popover.

- **AM-QA-006: Copied image file paste**
  - Copy a PNG/JPG/WebP image file in Finder, switch Airmark to Image mode, click Paste, and verify the file path is applied.

- **AM-QA-007: Copied image data paste**
  - Copy an image directly from ChatGPT/Safari/Preview, switch Airmark to Image mode, click Paste image, and verify Airmark saves and applies the pasted PNG.

## Regression Checklist

- App launch shows settings/setup only when setup is not complete.
- Overlay does not appear until `Confirm & Enable`.
- Overlay never focuses itself.
- Overlay is click-through.
- Overlay excludes the macOS menu bar work area.
- Settings can disable overlay immediately.
- Display selector updates overlay target after setup.
- Controls popover opens from the menu-bar icon and stays on the same display as the icon.
- Settings window stays open during drag/drop and only closes from its close button.
- Text size and text spacing use sliders and update the overlay live.
- Image mode hides text controls and supports file drop/choose plus copied image data/file paste.
