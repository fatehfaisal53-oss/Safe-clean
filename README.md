# SafeClean: Android Architecture & Permissions Compliance

SafeClean is architected specifically around modern Android security principles, adhering to targetSdk 35 (Android 15) and Google Play Developer Program Policies.

## 1. Storage Access Framework (SAF) vs MANAGE_EXTERNAL_STORAGE
* **Why MANAGE_EXTERNAL_STORAGE is avoided:**
  Google Play prohibits standard device cleaners from using the `MANAGE_EXTERNAL_STORAGE` all-files permission. Cleaner apps requesting it are routinely rejected unless they are bona fide file managers or backup solutions.
* **The SafeClean Solution:**
  SafeClean uses `ACTION_OPEN_DOCUMENT_TREE` (Storage Access Framework). When a user grants access to their Downloads or Media tree, Android issues a persistent URI permission token (`takePersistableUriPermission`). The app inspects and cleans via `DocumentsContract.deleteDocument`.

## 2. Transparent Heuristic App Security (No False "Antivirus" Claims)
* SafeClean does **not** claim to be a signature-based antivirus or malware remover.
* Instead, it exposes **transparent heuristic rule checks**:
  - Apps combining `BIND_ACCESSIBILITY_SERVICE` with overlay drawing (`SYSTEM_ALERT_WINDOW`).
  - Apps sideloaded from unknown package sources requesting SMS read permissions.
  - Apps mimicking system packages.
* Users can tap to open the official Android `ACTION_APPLICATION_DETAILS_SETTINGS` to inspect or uninstall.

## 3. Background Activity & Process Lifecycle
* Since Android 8.0 Oreo and Android 14 Foreground Service policies, apps cannot kill other apps using Linux `killProcess`. Attempting to do so causes immediate process recreation and higher battery drain.
* SafeClean displays running processes and memory via `ActivityManager`, and provides direct links to the official Battery Optimization settings.
