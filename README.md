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

About 15 seconds after launch, the application checks the official signed version policy while idle, without blocking startup or interrupting edits. A superseded version has a 14-day upgrade grace period. After its deadline, viewing and backup remain available, but data-changing operations are restricted; the official download page remains accessible. The latest version does not expire because of a single network failure. Failed checks retry with backoff. Upgrade through the official download page; installation-based automatic updates are not enabled.

## Safety and privacy

Tasks and settings stay on this computer. No account or task cloud sync is required, and version checks do not upload task contents. SHA256SUMS detects corrupted downloads; it is not a digital signature. Minisign policy signatures are not native platform code signatures or Apple notarization. Unsigned packages may trigger Windows SmartScreen or macOS Gatekeeper. Do not disable system-wide security protections. Back up before installation or upgrade. Save edits and exit before replacing program files; updates do not automatically restart the application.
