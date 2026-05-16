# Zebra RFID SDK Sample - AutoConnect

This application demonstrates how to use the Zebra RFID SDK to connect to RFID readers over USB or Bluetooth.

## Repository
- Remote: https://github.com/GelatoCookie/AutoSwitchConnect
- Local workspace: /Users/chucklin/StudioProjects/63_TC22AutoConnect/AutoSwitch


## Features
-   **Auto-Connection**: Automatically attempts to connect to available RFID readers on startup or when a device is attached.
-   **USB & Bluetooth Support**: Supports Zebra Sled readers over USB (CDC mode) and Bluetooth (BTLE/Classic).
-   **Inventory**: Perform RFID tag inventory with an edge-to-edge technical UI for high density data display.
-   **Robust Hardware Handling**: Improved startup stability with concurrency guards and transport-aware fallbacks.
-   **Visual Feedback**: Progress indicator (hourglass style) during inventory.
-   **Permissions**: Handles Bluetooth permissions required for discovery and connection on Android 12+.

## Getting Started
1.  Clone the repository.
2.  Open in Android Studio.
3.  Build and run on a Zebra device (e.g., TC22) or any Android device with Zebra RFID sleds.

## Version
- **1.0.3**: Hardened Bluetooth permission result handling, improved Bluetooth transport detection, synchronized instrumentation tests, and refreshed project documentation.
- **1.0.2**: Integrated discovery progress indicator for enhanced startup UX.
- **1.0.1**: Added automated transport discovery messaging on app startup.
- **1.0.0**: Production release with edge-to-edge UI, robust initialization, and technical messaging refinement.
- **Init**: Initial release with core functionality and fixes.

## History
- **1.0.3**: Permission and test-sync hardening. Requires both Android 12+ Bluetooth permissions before RFID init, improves Bluetooth transport classification, and aligns instrumentation tests with current APIs.
- **1.0.2**: Added a non-blocking startup discovery progress indicator.
- **1.0.1**: Added automatic transport discovery messaging on startup.
- **1.0.0**: Production release with UI refinement, stronger initialization gating, and clearer technical status messaging.
- **0.0.3**: Fixed startup permission recovery, cleaned up pre-check-in flows, and expanded device-backed permission coverage.
- **0.0.2**: Improved reconnect flow, transport-aware reader selection, and refreshed docs/flowcharts.
- **0.0.1**: Hardened lifecycle and permission handling, fixed dependency conflicts, and added initial test/doc coverage.

## Current Review Status (2026-05-16)
- Main Java compile check passed: `./gradlew :app:compileDebugJavaWithJavac`.
- Android test-source compile passed: `./gradlew :app:compileDebugAndroidTestJavaWithJavac`.
- Completed fixes:
  - Permission callback now requires both Android 12+ Bluetooth permissions before RFID init.
  - Bluetooth transport detection now uses active transport state with reader-name fallback for robust disconnect handling.
  - Instrumentation tests are synchronized with current app APIs/signatures.
