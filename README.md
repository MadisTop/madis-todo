# Madis Todo

**尚未发布。** 本仓当前只有分发规范与配置，没有可供下载的正式版本、安装包或自动更新服务；不提供占位下载地址。产品名 **Madis Todo**，发布者 **Madis**，应用标识 **`top.madis.todo`**。

## 三仓库与职责

| 仓库 | 职责与当前状态 |
| --- | --- |
| [madis-todo-source](https://github.com/MadisTop/madis-todo-source) | Rust + Slint 客户端、SQLite 存储、业务测试和实施计划；正式基础版开发中。 |
| [madis-todo-build](https://github.com/MadisTop/madis-todo-build) | 固定源码提交的四平台原生编译核验；目前仅手动核验骨架，不等于安装打包或正式发布。 |
| [madis-todo](https://github.com/MadisTop/madis-todo) | 产品说明、分发配置及未来正式发布入口；当前尚未发布，没有正式下载或更新服务。 |

三个仓库统一产品名 **Madis Todo**、发布者 **Madis**、标识 **`top.madis.todo`**，默认分支为 `main`。仓库描述、主题标签和主页应与各自职责及实际发布状态一致；产品入口统一指向分发仓，不将开发构建标记为正式发行。

## 平台范围

| 平台 | 架构 | 规划格式 | 当前边界 |
| --- | --- | --- | --- |
| Windows | x64 | NSIS `.exe`、MSI `.msi` | 当前用户 NSIS 在 source 单独推进；本仓未发布，安装/卸载与签名未验收，MSI 未完成 |
| Linux | x64 | `.AppImage`、`.deb`、`.rpm` | 打包、签名、安装与更新未验收 |
| macOS Intel | x86_64 | `.dmg`、`.app.tar.gz` | 应用包、Developer ID、Apple 公证与更新未验收 |
| macOS ARM | aarch64 | `.dmg`、`.app.tar.gz` | 应用包、Developer ID、Apple 公证与更新未验收 |

`release.config.json` 是初始机器可读规范；其中 `null` 和 `not-verified` 表示没有证据，不能当作发布事实。build 仓的原生可执行 Artifact **不是安装器，不是更新包**；编译成功也不能替代干净系统验收。Minisign 验签与平台原生签名/Apple 公证是不同门槛，不能相互冒充。

## 安装与数据约定（规范，未完整实现）

Windows 计划采用当前用户安装，程序目录为 `%LOCALAPPDATA%\Madis\Madis Todo`，Publisher 为 Madis，不默认要求管理员权限。用户数据始终与程序分离，按应用标识存放：

- Windows：`%LOCALAPPDATA%\top.madis.todo`
- macOS：`~/Library/Application Support/top.madis.todo`
- Linux：`$XDG_DATA_HOME/top.madis.todo`；未设置 XDG 时为 `~/.local/share/top.madis.todo`

卸载默认保留数据库、设置、备份和其他用户数据，不得递归删除上述目录。用户主动清理数据必须另行明确确认，不绑定普通卸载。当前用户 NSIS 即使仅本机编译通过，仍不等于真实安装、升级、退出交接或卸载已验证。

## 更新与发布约定（仅规范，未实现）

1. 只从未来正式公布的可信 HTTPS 发布位置获取元数据与包；当前没有发布入口，不猜测下载地址。下载到独立暂存区，完整下载并校验后才允许进入安装。
2. 必须用应用内受信任的 Minisign 公钥验证包的 detached 签名及绑定元数据的签名。不能信任与包一同下载但未经独立信任链认证的新公钥，不能只比对可被替换的 SHA-256。密钥供应、轮换和验签代码当前均未实现，本仓不放任何密钥值或伪造 `.sig`。
3. 签名元数据须绑定 `top.madis.todo`、目标产品版本、操作系统、CPU 架构、包格式、字节数和 SHA-256；这些值必须与已验签包、当前产品及目标平台全部一致。拒绝缺字段、错产品、错架构、损坏、验签失败或不受信任的降级；不能在不同安装格式间默默切换。
4. 安装前提示并取得用户同意；应用必须先保存待写事务、释放数据库与进程锁、真正退出（包括浮窗/托盘），再由独立安装/更新进程接手。失败、取消或退出超时都不得强行覆盖正在使用的程序；用户数据不参与包替换或卸载清理。安装器退出码与重新启动须单独验收。
5. 发布顺序固定为：生成并验证各平台包 → 生成真实签名与绑定元数据 → 上传包及签名 → 回读核对名称、字节数、哈希与验签 → **最后发布 `latest.json`**（及其真实签名）。任何失败不得把不完整版本标成 latest；未来元数据发布还需处理一致性、重试与回滚。当前不创建该文件、不建 Release，不承诺自动更新可用。

正式发布前还须逐平台完成干净系统安装/启动/升级/卸载保留数据回归、原生签名及必要公证、更新失败/中断/错包测试、实际第三方许可核查。本仓规范不代表这些门槛已经完成。
