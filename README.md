# Madis Todo

Madis Todo：基于 Rust、Slint 和 SQLite 的轻量离线待办应用，支持任务管理、计划日期、重复任务、日历搜索与备份恢复。

发布者 **Madis**，应用标识 **`top.madis.todo`**。

## 支持平台

| 平台 | 安装包格式 |
| --- | --- |
| Windows x64 | NSIS `.exe`、MSI `.msi` |
| Linux x64 | `.AppImage`、`.deb`、`.rpm` |
| macOS Intel | `.dmg`、`.app.tar.gz` |
| macOS Apple Silicon | `.dmg`、`.app.tar.gz` |

Linux 图形会话使用 X11 或 XWayland。选择与系统及处理器架构匹配的安装包，各下载入口提供的格式以对应版本的附件列表为准。

## 下载入口

- [GitHub 发布页面](https://github.com/MadisTop/madis-todo/releases)
- [Gitee 发布页面](https://gitee.com/madis/madis-todo/releases)

从发布页面选择版本并阅读该版本的变更说明，不使用来源不明的转载包。

## 安装与使用

- Windows：运行 `.exe` 或 `.msi`，按引导安装到当前用户目录；升级时沿用原安装格式。
- Linux：为 AppImage 添加执行权限后运行，或使用系统包管理器安装 DEB/RPM。
- macOS：打开 DMG 并将应用复制到应用目录，或解压应用归档后运行。
- 在主窗口或置顶浮窗中新增和编辑任务；计划日期、重复规则、主题及备份恢复可从对应面板或设置中操作。

## 数据位置与备份

Windows 程序目录为 `%LOCALAPPDATA%\Madis\Madis Todo`。任务和设置与程序分开存放：

- Windows：`%LOCALAPPDATA%\top.madis.todo`
- macOS：`~/Library/Application Support/top.madis.todo`
- Linux：`$XDG_DATA_HOME/top.madis.todo`；未设置有效绝对 XDG 路径时为 `~/.local/share/top.madis.todo`

可在设置中备份和恢复任务、重复系列及设置。卸载默认保留数据库、设置和备份；恢复前先保存当前数据的备份。

## 安装安全与隐私

安装或升级前请备份已有数据。未经原生代码签名或 Apple 公证的安装包可能受到 Windows SmartScreen 或 macOS Gatekeeper 拦截；不要关闭全局系统安全策略。`SHA256SUMS` 仅用于检测下载损坏，不是数字签名。Minisign 更新签名不能代替平台原生签名或 Apple 公证。

任务和设置保存在本地，不需要账号，不提供任务云同步。检查更新与下载安装包需要联网，但不上传任务内容。使用内置更新入口时，安装前须保存编辑内容并真正退出应用；安装后不自动重启。
