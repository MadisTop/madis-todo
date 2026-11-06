# Madis Todo

**尚未发布。** 本仓当前只有分发规范与配置，没有可供下载的正式版本、安装包或自动更新服务；不提供占位下载地址。产品名 **Madis Todo**，发布者 **Madis**，应用标识 **`top.madis.todo`**。

## 三仓库与职责

| 仓库 | 职责与当前状态 |
| --- | --- |
| [madis-todo-source](https://github.com/MadisTop/madis-todo-source) | Rust + Slint 客户端、SQLite 存储、业务测试和实施计划；离线主体功能和数据边界修复已实现；原生验收、安全更新及发行未完成。 |
| [madis-todo-build](https://github.com/MadisTop/madis-todo-build) | 固定源码/配置 SHA 的四平台编译、原生打包与 Artifact 回读代码；尚无四平台 CI 成功证据，非正式发布。 |
| [madis-todo](https://github.com/MadisTop/madis-todo) | 产品说明、分发配置及未来正式发布入口；当前尚未发布，没有正式下载或更新服务。 |

三个 GitHub 仓库统一产品名 **Madis Todo**、发布者 **Madis**、标识 **`top.madis.todo`**，默认分支为 `main`；Gitee 两仓的目标分支同为 `main`，但尚未初始化。仓库描述、主题标签和主页应与各自职责及实际发布状态一致；产品入口统一指向分发仓，不将开发构建标记为正式发行。

## 平台范围

| 平台 | 架构 | 规划格式 | 当前边界 |
| --- | --- | --- | --- |
| Windows | x64 | NSIS `.exe`、MSI `.msi` | NSIS/MSI 模板与 CI 调用已补；本仓未发布，原生打包/安装卸载/签名均未验收 |
| Linux | x64 | `.AppImage`、`.deb`、`.rpm` | AppImage/DEB/RPM 打包与回读代码已补；真实打包、签名、安装与更新未验收 |
| macOS Intel | x86_64 | `.dmg`、`.app.tar.gz` | DMG/应用归档模板及回读已补；真实打包、Developer ID、Apple 公证与更新未验收 |
| macOS ARM | aarch64 | `.dmg`、`.app.tar.gz` | 应用包、Developer ID、Apple 公证与更新未验收 |

`release.config.json` 是机器可读门禁规范；其中 `null` 和 `not-verified` 表示没有证据，不能当作发布事实。build 仓的原生可执行 Artifact **不是安装器，不是更新包**；编译成功也不能替代干净系统验收。Minisign 验签与平台原生签名/Apple 公证是不同门槛，不能相互冒充。

## 安装与数据约定（规范，未完整实现）

Windows 计划采用当前用户安装，程序目录为 `%LOCALAPPDATA%\Madis\Madis Todo`，Publisher 为 Madis，不默认要求管理员权限。用户数据始终与程序分离，按应用标识存放：

- Windows：`%LOCALAPPDATA%\top.madis.todo`
- macOS：`~/Library/Application Support/top.madis.todo`
- Linux：`$XDG_DATA_HOME/top.madis.todo`；未设置 XDG 时为 `~/.local/share/top.madis.todo`

卸载默认保留数据库、设置、备份和其他用户数据，不得递归删除上述目录。用户主动清理数据必须另行明确确认，不绑定普通卸载。当前用户 NSIS 即使仅本机编译通过，仍不等于真实安装、升级、退出交接或卸载已验证。

## 更新与发布约定（基础库已接入，生产更新关闭）

安全更新相关五项依赖已批准。客户端 `update_verify`、`download`、`update.rs` 基础库已通过 `lib` 正式接入编译，目前仅为 **64 MiB 受限演练**，生产更新策略禁用，仍缺真实受信公钥与端点。尚无桌面 UI 接线、强更与安装交接，也未完成真实签名和四平台验收；不能把基础库编译通过当作运行态安全更新完成。

1. 只从未来正式公布的可信 HTTPS 发布位置获取元数据与包；当前没有发布入口，不猜测下载地址。下载到独立暂存区，完整下载并校验后才允许进入安装。
2. 必须用应用内受信任的 Minisign 公钥验证包的 detached 签名及绑定元数据的签名。不能信任与包一同下载但未经独立信任链认证的新公钥，不能只比对可被替换的 SHA-256。客户端验签基础库已接入，但真实受信密钥供应、轮换及真实签名验收尚未完成；本仓不放任何密钥值或伪造签名。已批准安全子集严格限定 `signatureMode=minisign-legacy-ed`：签名端必须显式 `minisign -S -l` 使用原始 `Ed`，拒绝默认 `ED`/Blake2b 预哈希签名。build 的 `verifyDetached` 回读签名 packet，校验原始 `Ed`、key ID 与容器结构后再调用真实 Minisign 验签；仅公钥为 `Ed` 或默认 `minisign -V` 成功不代表客户端兼容。
3. 签名元数据须绑定 `top.madis.todo`、目标产品版本、source/build/distribution 三个完整 SHA，以及每包目标、操作系统、CPU 架构、格式、精确名称、字节数、SHA-256、下载 URL 和独立签名 URL；这些值必须与已验签包、当前产品及目标平台全部一致。拒绝缺字段、错产品、错架构、损坏、验签失败或不受信任的降级；不能在不同安装格式间默默切换。
4. 安装前提示并取得用户同意；应用必须先保存待写事务、释放数据库与进程锁、真正退出（包括浮窗/托盘），再由独立安装/更新进程接手。失败、取消或退出超时都不得强行覆盖正在使用的程序；用户数据不参与包替换或卸载清理。安装器退出码与重新启动须单独验收。
5. 发布顺序固定为：生成并验证各平台包 → 生成真实签名与绑定元数据 → 上传包及签名 → 回读核对名称、字节数、哈希与验签 → **最后发布 `latest.json`**（及其真实签名）。任何失败不得把不完整版本标成 latest；未来元数据发布还需处理一致性、重试与回滚。当前不创建该文件、不建 Release，不承诺自动更新可用。`updates.state=foundation-implemented-disabled`、`enabled=false`、`updaterDependencyApproved=true`、公钥与端点 `null` 明确区分基础库已接入与生产更新未启用，不是可被客户端使用的占位配置。`release.state=unpublished`、`latestPublished=false`、`endToEndVerified=false` 及四平台 `updater=not-implemented` 保持不变，平台运行态更新仍未交付/验收。

正式发布前还须逐平台完成干净系统安装/启动/升级/卸载保留数据回归、原生签名及必要公证、更新失败/中断/错包测试、实际第三方许可核查。本仓规范不代表这些门槛已经完成。

## Gitee 私有仓状态

目标 `madis/madis-todo-source` 与 `madis/madis-todo` 已建为私有仓。2026-11-06 已按授权首次推送并回读两个 `main`；`gitee` remote 保留，GitHub `origin` 未变；`repositories.gitee.provisioning=private-main-initialized`。这只表示仓库和主分支存在，不表示分支保护、CI 自动同步或 Gitee 产品发布已验收。

源码镜像只面向已有 `main` 的快进更新，不创建分支、不 force、不更改可见性；产品仓不由源码同步写入。每 12 小时及手动同步的 CI 定义不等于已启用，须完成初始化、禁止删除保护及凭据最小权限核验后才能开启门禁。

## 实施与外部前置条件

build 仓提供逐包解包、Artifact 上传后下载回读、真实 Minisign 命令入口及严格门禁保护的现有空草稿部署脚本；其网络测试为模拟，不代表真实上传/签名/正式发布已完成。部署时包及签名先上传并回读，最后发布元数据；当前门禁会在联网前拒绝正式部署，不生成未验证的 updater 元数据。

候选构建使用独立 `SOURCE_DEPLOY_KEY`、`DISTRIBUTION_DEPLOY_KEY` 和官方 `SOURCE_KNOWN_HOSTS`，两把部署密钥均由 GitHub 对应私有仓登记为只读；已通过本机 SSH 和真实 CI 检出验证。不使用普通 job token 跨私有仓，不复用 pk/json 的私钥。`RPM_LICENSE=LicenseRef-Proprietary` 已配置，Linux 使用固定官方 AppImage 工具并校验摘要。源码定时镜像仍需专用 Gitee 写凭据及保护规则核验，不能把构建的只读 SSH 密钥当作 HTTP token。生产更新采用已批准的独立 HTTPS 只读分发网关方向，但未提供/部署真实网关和独立签名信任根，运行态更新与安装助手尚未交付。


## 收尾状态

2026-11-06 收尾：离线主体、重复标题/备份恢复/超量补齐/数据库别名等数据边界修复及发布门禁已回归；跨平台原生窗口、完整自动更新、安装升级及 Gitee 分发仍未完成。本仓保持未发布，不提供虚假下载/签名/验收结论。收尾顺序见源码仓 docs/收尾验收清单.md。

当前收尾证据以源码仓《收尾验收清单》为准。两个 Gitee 仓的 main 已初始化并可普通快进推送。四目标候选流水线正在按真实失败日志修复，不能将触发成功当作编译/安装验收成功。私有测试通道仅允许成功四目标候选新建草稿或显式预发布，绝不生成 latest、更新签名或改动仓库可见性；不能冒充正式安全更新交付。
