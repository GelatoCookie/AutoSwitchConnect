# Release Notes

## 1.0.0 (2026-04-22)

- Tag: `1.0.0`
- Title: Production Release - UI Refinement, Robust Initialization, and Technical Messaging

### Highlights

- **Edge-to-Edge UI**: Modified tag list items to span the full width of the application by removing horizontal margins and corner radii, providing a more professional technical appearance.
- **Robust Initialization**: Implemented an initialization gate in `RFIDHandler` to prevent concurrent SDK initialization attempts and potential startup crashes when hardware is not present.
- **Technical Messaging Refinement**: Performed a global audit of UI status messages and logs, standardizing on precise technical terminology (e.g., "Transport Latency", "USB/Bluetooth Transport", "Detected (Disconnected)").
- **Build Optimization**: Updated ProGuard configuration to use `proguard-android-optimize.txt` for improved R8 optimizations and to resolve deprecation warnings.

### Technical Changes

- Main logic:
  - `app/src/main/java/com/zebra/rfid/demo/sdksample/RFIDHandler.java`: Added `isInitializing` guard, improved error handling in async init, and refined log/UI strings.
  - `app/src/main/java/com/zebra/rfid/demo/sdksample/MainActivity.java`: Improved lifecycle handling in `onPostResume`.
- Layouts:
  - `app/src/main/res/layout/tag_list_item.xml`: Removed margins and elevation for edge-to-edge display.
  - `app/src/main/res/layout/activity_main.xml`: Refined constraints for `lstTags`.
- Build:
  - `app/build.gradle`: Updated ProGuard file.

### Validation Summary

- Verified edge-to-edge layout on device.
- Stress-tested startup without reader hardware to ensure no crashes.
- Confirmed technical string accuracy in UI.

## v0.0.3 (2026-04-21)

- Title: Startup permission recovery, pre-check-in cleanup, release prep

### Highlights

- Fixed the Android 12+ Bluetooth startup permission path so RFID init resumes after the user grants access.
- Removed the false startup Bluetooth permission warning that appeared before the runtime permission prompt was answered.
- Added device-backed coverage for the partial permission-result callback and the pending startup permission-request path.
- Moved remaining permission/status strings into resources and migrated BuildConfig generation to module-level Gradle config.
- Performed a low-risk `MainActivity` cleanup to reduce review noise before check-in and tagging.

### Technical Changes

- Main logic:
  - `app/src/main/java/com/zebra/rfid/demo/sdksample/MainActivity.java`
- Tests:
  - `app/src/androidTest/java/com/zebra/rfid/demo/sdksample/ExampleInstrumentedTest.java`
- Build config:
  - `app/build.gradle`
  - `gradle.properties`
- Resources:
  - `app/src/main/res/values/strings.xml`
- Docs:
  - `README.md`
  - `design.md`
  - `RELEASE.md`

### Validation Summary

- `./android_tasks.sh build`
- `./android_tasks.sh connected-test`

The debug build passed after the pre-check-in cleanup, and the connected test suite passed with 14/14 device-backed tests.

## v0.0.2 (2026-04-21)

- Title: Reconnect flow follow-up, transport-aware selection, docs refresh

### Highlights

- Removed the last UI-blocking reconnect behavior from the Bluetooth dialog path.
- Completed Zebra USB permission handling and transport-aware preferred reader selection.
- Added device-backed coverage for vendor-id matching and USB-vs-Bluetooth reader-name selection.
- Refreshed README, design notes, and Mermaid flowcharts so they match the current implementation.
- Reduced complexity in menu handling and scanner SDK setup with small helper-method refactors.

### Technical Changes

- Main logic:
  - `app/src/main/java/com/zebra/rfid/demo/sdksample/MainActivity.java`
  - `app/src/main/java/com/zebra/rfid/demo/sdksample/RFIDHandler.java`
- Tests:
  - `app/src/androidTest/java/com/zebra/rfid/demo/sdksample/ExampleInstrumentedTest.java`
- Docs:
  - `README.md`
  - `design.md`
  - `flowchart TD.mmd`
  - `graph TD.mmd`
- Tooling:
  - `android_tasks.sh`

### Validation Summary

- `./android_tasks.sh build`
- `./android_tasks.sh connected-test`
- Mermaid syntax validation for the updated flowchart

The build and connected test suite passed after these updates.

## v0.0.1 (2026-04-19)

- Tag: `v0.0.1`
- Commit: `d50e325`
- Title: Harden RFID lifecycle, permissions, tests, and docs

### Highlights

- Hardened Android Bluetooth permission handling for Android 12+.
- Fixed USB broadcast receiver lifecycle registration/unregistration behavior.
- Added null-safe USB attach handling.
- Improved BT/USB transport state synchronization in RFID connection lifecycle.
- Added instrumentation tests for permission callback behavior and receiver lifecycle.
- Fixed duplicate Zebra API dependency conflict by pinning one API3 AAR.
- Updated project documentation and diagrams.

### Technical Changes

- Main logic changes:
  - `app/src/main/java/com/zebra/rfid/demo/sdksample/MainActivity.java`
  - `app/src/main/java/com/zebra/rfid/demo/sdksample/RFIDHandler.java`
- Tests:
  - `app/src/androidTest/java/com/zebra/rfid/demo/sdksample/ExampleInstrumentedTest.java`
- Build config:
  - `app/build.gradle`
- Resources:
  - `app/src/main/res/values/strings.xml`
- Docs and flow:
  - `README.md`
  - `design.md`
  - `flowchart TD.mmd`
  - `graph TD.mmd`
  - `.vscode/tasks.json`
  - `android_tasks.sh`

### Validation Summary

- `./gradlew :app:compileDebugJavaWithJavac`
- `./gradlew :app:compileDebugAndroidTestJavaWithJavac`
- `./gradlew :app:assembleDebug --console=plain`
- `./auto_build_run.sh`

All commands above completed successfully in the release branch state.

### Notes

- This tag was created after a broad commit that included source/doc changes and workspace-generated files.
- For the next release, consider splitting generated artifacts from source changes to keep release diffs clean.
