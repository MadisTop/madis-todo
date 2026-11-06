# Madis Todo

**尚未发布。** 产品名 **Madis Todo**，发布者 **Madis**，应用标识 **`top.madis.todo`**。三个 GitHub 仓库及两个 Gitee 仓库当前均保持私有，测试版与正式版分开管理。

Madis Todo 是 Rust + Slint 开发的紧凑待办软件，不依赖 WebView。主窗口与始终置顶的浮窗共用数据，支持行内编辑、计划日期、重复任务、日历搜索、明暗主题、贴边收起、备份与恢复。

## 平台与当前状态

| 平台 | 已构建并回读的候选格式 | 尚未验收的边界 |
| --- | --- | --- |
| Windows x64 | NSIS `.exe`、MSI `.msi` | 原生签名、干净系统安装/升级/卸载及退出更新 |
| Linux x64 | `.AppImage`、`.deb`、`.rpm` | 原生签名、运行依赖及安装/升级/卸载 |
| macOS Intel | `.dmg`、`.app.tar.gz` | Developer ID、Apple 公证及安装更新 |
| macOS ARM | `.dmg`、`.app.tar.gz` | Developer ID、Apple 公证及安装更新 |

四个平台的九个候选包已经在同一轮 CI 完成打包、上传和下载回读。候选构建成功不是正式发布，也不等于所有实机交互已经验收。

## 安装与数据位置

Windows 采用当前用户安装，程序目录为 `%LOCALAPPDATA%\Madis\Madis Todo`。数据与程序分开存放：

- Windows：`%LOCALAPPDATA%\top.madis.todo`
- macOS：`~/Library/Application Support/top.madis.todo`
- Linux：`$XDG_DATA_HOME/top.madis.todo`；未设置 XDG 时为 `~/.local/share/top.madis.todo`

卸载默认保留数据库、设置和备份。用户数据不参与程序包替换，不因普通卸载而删除。

## 更新与签名

本项目已建立独立 Minisign 签名密钥，并将公开信任根配置在项目中；私钥只保存在加密凭据存储，不进入源码、安装包或本仓库。签名严格使用 `minisign -S -l` 原始 Ed 模式，同时验证消息与受信注释，拒绝未批准的签名模式。Minisign 更新签名不能代替平台原生签名或 Apple 公证。

当前 `updates.state=foundation-implemented-disabled`、`enabled=false`：桌面后台更新、退出后的安装助手与真实只读分发服务仍在开发，尚未启用。不会向普通客户端内嵌 GitHub/Gitee 私有访问 Token，也不会将私有仓地址误当作匿名可用更新服务。

发布顺序为：验证各平台包 → 生成真实签名与绑定元数据 → 上传包和签名 → 回读名称、大小、摘要并验签 → **最后发布**更新元数据。任何失败都不更新稳定通道。安装前须保存数据、真正退出并释放锁，再交由独立安装进程处理，不自动重启应用。

`release.config.json` 是机器可读配置：`packaging=verified` 仅表示上述候选打包回读；`not-verified` 与 `not-implemented` 分别表示尚未验收和尚未实现，不能混同。公钥非空不表示生产更新已经启用。

## Gitee 镜像

私有源码镜像和私有产品仓的 `main` 均已初始化。源码镜像按每 12 小时及手动触发设计，仅向已有 `main` 快进，不 force、不删除分支、不更改仓库可见性。自动镜像须在专用凭据登记、分支保护回读与真实同步成功后才算交付。

产品镜像参照 Madis 其它项目管理：标签、标题与更新说明一致；Gitee 排除 `.AppImage` 及其独立签名，并生成不引用排除文件的校验清单。产品发布与客户端下载链路尚未完成验收。
