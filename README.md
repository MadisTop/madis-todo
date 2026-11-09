# Madis Todo

English | [简体中文](README.zh-CN.md)

Madis Todo is a lightweight offline task manager built with Rust, Slint and SQLite. It supports planned dates, recurring tasks, calendar search, backups and restore.

Publisher: **Madis**. Application identifier: **top.madis.todo**.

## Downloads and platforms

Use the official [GitHub releases](https://github.com/MadisTop/madis-todo/releases) or [Gitee releases](https://gitee.com/madis/madis-todo/releases).

| Platform | Packages |
| --- | --- |
| Windows x64 | NSIS .exe, MSI .msi |
| Linux x64 | AppImage, DEB, RPM |
| macOS Intel | DMG, app.tar.gz |
| macOS Apple Silicon | DMG, app.tar.gz |

Linux requires X11 or XWayland. Available formats depend on the release; Gitee does not mirror AppImage files. Package filenames use madis-todo_<version>_<target>.

## Install and use

Run the Windows installer, install a Linux package or make the AppImage executable, or copy the macOS application into Applications. Keep the same installer format when upgrading Windows. Create and edit tasks in the main window or floating window; use settings for dates, recurrence, themes and backups.

The Windows default install directory is %LOCALAPPDATA%\Programs\Madis\Todo. Existing valid installer registrations are respected. MSI cannot automatically recover an unregistered historical custom directory; provide INSTALLFOLDER explicitly when needed.

## Local data and backups

| Platform | Database |
| --- | --- |
| Windows | %LOCALAPPDATA%\Madis\Todo\Data\todo.sqlite3 |
| macOS | ~/Library/Application Support/Madis/Todo/Data/todo.sqlite3 |
| Linux | $XDG_DATA_HOME/Madis/Todo/Data/todo.sqlite3, or ~/.local/share/Madis/Todo/Data/todo.sqlite3 |

Settings shows the actual database location. Back up before restoring or upgrading. Normal uninstall preserves user data, settings and backups. Do not delete shared Madis directories.

Windows and Linux support portable mode with --portable or a valid portable.json containing {"version":1} next to the executable. Its Data directory stays next to the actual executable (next to the outer AppImage on Linux), independent of the working directory. Invalid markers fail explicitly. macOS portable mode is not supported. Close the application before moving the full program and Data directory. Portable mode never starts installation-based updates. --data-file selects a separate database and cannot be combined with portable mode. Existing legacy data is not moved or merged automatically.

## Version checks and upgrade grace period

Starting with v0.1.3, the application checks signed update metadata about 15 seconds after launch while idle. It downloads and verifies the matching package in the background, saves tasks and window state, exits to install, then restarts automatically with the same data location. Active edits and backup operations defer installation; failed saves keep the application open. Windows automatic installation supports NSIS installations; MSI must be upgraded manually with MSI. Linux DEB/RPM use the original package manager; portable mode never starts an installer.

A superseded version expires after one UTC calendar month, with month-end clipping; existing signed historical deadlines are never extended. Offline verification has a separate 14-day grace period, refreshed only by successful online signed-policy verification. Viewing, backup and the official download page remain available after restrictions begin. A single failed network check does not immediately expire the latest version; failures retry with backoff.

Versions v0.1.1 and v0.1.2 did not enable installation-based updates. Install v0.1.3 manually once to receive subsequent signed automatic updates.

## Safety and privacy

Tasks and settings stay on this computer. No account or task cloud sync is required, and version checks do not upload task contents. SHA256SUMS detects corrupted downloads; it is not a digital signature. Minisign policy signatures are not native platform code signatures or Apple notarization. Unsigned packages may trigger Windows SmartScreen or macOS Gatekeeper. Do not disable system-wide security protections. Back up before installation or upgrade. The automatic updater verifies independent Minisign signatures for metadata and packages before installation. Native code signing, notarization and real-device upgrade acceptance are separate checks.
