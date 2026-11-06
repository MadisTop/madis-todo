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

Windows 默认程序目录为 `%LOCALAPPDATA%\Programs\Madis\Todo`。NSIS 沿用已登记的合法安装目录。MSI 仅自动沿用含 `madis-todo-desktop.exe` 的已登记地址，或旧默认 `%LOCALAPPDATA%\Madis\Madis Todo`；旧 MSI 未记录的自定义地址不能自动恢复，需用 `msiexec /i 安装包.msi INSTALLFOLDER="原安装目录"` 显式指定且该目录包含上述 EXE，否则拒绝升级，不静默换址。应用标识始终为 `top.madis.todo`。安装模式数据库路径：

| 平台 | 数据库 |
| --- | --- |
| Windows | `%LOCALAPPDATA%\Madis\Todo\Data\todo.sqlite3` |
| macOS | `~/Library/Application Support/Madis/Todo/Data/todo.sqlite3` |
| Linux | 有效绝对 `$XDG_DATA_HOME/Madis/Todo/Data/todo.sqlite3`，否则 `~/.local/share/Madis/Todo/Data/todo.sqlite3` |

`Data` 的父目录是产品数据基目录，`State`、`Cache`、`Logs` 是其按需创建的子目录；更新临时数据只放 `Cache/updates`，不混入数据库。本产品为原生 Slint，无 WebView 数据目录。设置中可查看实际数据库位置并打开所在目录。

可在设置中备份和恢复任务、重复系列及设置。卸载默认保留数据库、设置和备份，不递归删除 Madis 共享父目录；恢复前先保存当前数据的备份。不自动探测旧 identifier 路径或搬迁用户目录。

### 便携使用（仅 Windows / Linux）

在可执行文件旁手工创建 `portable.json`，内容严格为 `{"version":1}`（允许 JSON 空白，不允许未知/重复字段或其它版本），或启动时添加 `--portable`。程序不会自动创建 marker；marker 存在但无效/不可读会报错，不会悄悄改用用户目录。macOS 明确不支持便携模式：开关与 marker 均会拒绝，避免向签名 `.app` 包内部写入数据；macOS 安装模式的数据路径不受影响。

- 数据根为**实际可执行文件所在目录的 `Data`**，数据库为 `Data/todo.sqlite3`；`State`、`Cache`、`Logs` 都位于这个 Data 内，不依赖启动时的工作目录。
- 选择可写目录；Linux AppImage 使用有效绝对 `APPIMAGE` 指向的外层启动文件作为锚点，marker 和 Data 放在外层包旁，不写入只读临时挂载；普通 Linux 二进制使用自身目录。
- 退出应用后连同程序和完整 Data 移动；自启动保留 `--portable`，移动后需重新登记启动项。
- 便携模式仍可检查更新并打开官方下载页，但不下载/调用 NSIS、MSI 等安装型 Updater；手动更新程序时保留完整 Data。
- `--data-file <路径>` 仍可指定独立数据库，相对路径按工作目录解析；其自启动保留解析后的绝对数据路径。该参数与开关或 marker 便携模式同时指定时明确拒绝冲突。

例如：`madis-todo-desktop.exe --portable`。不附加 `--portable` 且无 marker 时使用安装模式路径。

### 一次性迁移旧数据

迁移前先备份已有数据，将支持新布局的新版程序安装/解压就绪，并退出新旧 Todo；不要先启动新版创建目标库。应用不自动迁移或合并旧数据，需要迁移时请联系维护者按需提供一次性迁移工具。迁移完成后只使用新版，**不要再启动旧版**，避免新旧数据分叉。

## 安装安全与隐私

安装或升级前请备份已有数据。未经原生代码签名或 Apple 公证的安装包可能受到 Windows SmartScreen 或 macOS Gatekeeper 拦截；不要关闭全局系统安全策略。`SHA256SUMS` 仅用于检测下载损坏，不是数字签名。Minisign 更新签名不能代替平台原生签名或 Apple 公证。

任务和设置保存在本地，不需要账号，不提供任务云同步。检查更新与下载安装包需要联网，但不上传任务内容。使用内置更新入口时，安装前须保存编辑内容并真正退出应用；安装后不自动重启。
