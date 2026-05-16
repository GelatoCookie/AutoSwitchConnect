# RFID Connection Design Document

## Overview
This document describes the connection management logic implemented in `RFIDHandler.java` for the AutoSwitch project. The primary goal is to provide a robust, thread-safe mechanism for connecting to Zebra RFID readers (USB and Bluetooth) while preventing the common `java.lang.IllegalStateException: Already running` crash.

## State Management
We use `AtomicBoolean` flags to track and control the connection lifecycle across multiple threads (UI thread and a single-threaded `workQueue`).

| Flag | Purpose |
| :--- | :--- |
| `isInitializing` | Prevents multiple concurrent SDK initialization flows (`initializeReaders`). |
| `isConnectionQueued` | Ensures only one connection task is pending in the `workQueue`. |
| `isConnecting` | Prevents concurrent execution of the actual `reader.connect()` API. |
| `bIsReading` | Tracks the inventory state of the reader. |

## Key Components

### 1. Initialization (`InitSDK`)
- Checks if initialization is already in progress.
- If `readers` object is null, starts `initializeReadersAsync`.
- If `readers` exists, proceed straight to `connectReader`.

### 2. Connection Queuing (`connectReader`)
- Checks `isConnectionQueued` and `isConnecting`.
- If either is true, the request is ignored to prevent redundant tasks.
- If not, sets `isConnectionQueued = true` and posts `connectReaderAsync` to the work queue.

### 3. Connection Execution (`connectMethod`)
- Uses `isConnecting.compareAndSet(false, true)` for thread-safe entry.
- Performs the actual `localReader.connect()` call.
- **Recovery Logic**: If `connect()` throws an "Already running" exception, it attempts a surgical `disconnect()` followed by a second `connect()` attempt.
- Configures reader events and tones upon success.

### 4. Disposal and Disconnect
- `disconnect()`: Removes listeners and calls `reader.disconnect()`.
- `dispose()`: Calls `disconnect()`, cleans up the `readers` object, and resets all atomic flags to `false`.

## Error Handling
- **IllegalStateException (Already running)**: Handled by intercepting the exception in `connectMethod` and attempting a reset (disconnect/reconnect).
- **Transport Transitions**: When a reader disappears, `dispose()` is called to ensure a clean slate before attempting to switch transports (e.g., USB to Bluetooth).

## Code Review Follow-Up (2026-05-15)

### Resolved
- **Permission callback now validates both Android 12+ Bluetooth permissions**:
	`MainActivity.onRequestPermissionsResult` now checks permission results by permission name and requires both `BLUETOOTH_SCAN` and `BLUETOOTH_CONNECT` before RFID initialization.

- **Bluetooth transport flagging is no longer only `+` name-pattern based**:
	`RFIDHandler.updateTransportFlagsFromReader` now also uses active transport state and includes `RFD8500` naming fallback to improve Bluetooth disconnection-path reliability.

- **Instrumentation test compile drift fixed**:
	`ExampleInstrumentedTest` was aligned with current APIs/signatures (removed obsolete activity method references and fixed `onResume` override signature).

### Validation Snapshot
- `./gradlew :app:compileDebugJavaWithJavac` -> **PASS**
- `./gradlew :app:compileDebugAndroidTestJavaWithJavac` -> **PASS**
