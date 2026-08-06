---
name: jieli-headless-build
description: Use when headlessly building, rebuilding, packaging, downloading, or flashing Jieli AC79/AC7916 Code::Blocks .cbp projects on Windows, especially when post-build download.bat, Device Offline/-11, stale artifacts, or an already-running GUI may hide failure.
metadata:
  version: "1"
  upstream: "https://github.com/xiaohonghua661/xhh-skills"
  branch: "yueting"
  path: "嵌入式/jieli-headless-build/SKILL.md"
---

# 杰理 AC79 无头编译与烧录

## 核心原则

默认只编译。只有用户明确要求烧录，并且检测到目标硬件或升级设备时，才运行下载链。退出码 0、固件文件存在都不能单独证明成功。

## 先检查

1. 定位 `codeblocks.exe`、目标 `.cbp` 和 `Release` target；读取 `.cbp`，不要猜工程结构。
2. 检查 target 的 pre/post-build，重点搜索 `download.bat`、`isd_download`、`fw_add`、`ufw_maker` 和 `<Mode after="always" />`。
3. 记录运行前 `sdk.elf`、`app.bin`、`jl_isd.fw`、`jl_isd.ufw` 的存在性、大小、修改时间；需要区分同尺寸产物时再算 SHA-256。
4. 只使用本机帮助或实测支持的 Code::Blocks 参数；不要猜 `--no-ipc`、`--no-dde` 等参数。

## 只编译（默认）

官方 AC79 工程可能把 `download.bat sdk` 设为始终执行的 post-build。纯编译时：

1. 先备份 `.cbp` 并记录其 SHA-256。
2. 仅临时禁用调用下载脚本的 `<Add after="...download.bat sdk" />` 及其配对的 `<Mode after="always" />`；不要删除其他 build 命令。
3. 在 `finally` 等价的收尾路径恢复原 `.cbp`，并核对恢复后的 SHA-256。当前若本来就是临时 SDK 副本，就在副本操作；不要为了纯编译再复制一份完整 SDK。
4. 已有 GUI 实例也始终带 `--multiple-instance`：

```powershell
$cbp = 'C:\path\to\project.cbp'
& 'C:\Program Files\CodeBlocks\codeblocks.exe' `
  /na /nd --multiple-instance --no-splash-screen `
  --rebuild '--target=Release' $cbp
```

编译成功必须同时具备：进程已结束且状态为 0、日志含 `0 error(s), 0 warning(s)` 并完成链接、`sdk.elf` 为本轮新产物。缺一项就报告“未证明成功”。

## 烧录（显式请求才执行）

1. 确认用户要求的是 `flash/download/烧录`，并检查杰理强制升级工具、驱动/接线及升级模式。设备离线时停止，不用重复下载碰运气。
2. 保留旧产物证据或先移到明确的任务临时目录，避免把残留文件当成本轮结果。
3. 在 SDK 对应下载目录运行工程生成的 `download.bat sdk`，完整保存 stdout/stderr；不要自行重写官方下载链。
4. 日志出现 `Device Offline` 或 `-11`，立即判定烧录未成功。杰理脚本可能只 `echo %errorlevel%`，最后无条件 `exit /b 0`，所以批处理状态 0 不可信。
5. `app.bin`、`jl_isd.fw`、`jl_isd.ufw` 的新时间戳只证明打包链运行；真实烧录还必须看到设备完成下载，并从串口启动日志核对新固件版本。无法取得串口或版本证据时写“Flash 未验证”。

## 结果分级

| 可声明结果 | 最低证据 |
|---|---|
| 编译成功 | 构建日志无错误、完成链接、本轮新 `sdk.elf` |
| 固件打包成功 | 本轮新 `app.bin`、`jl_isd.fw`、`jl_isd.ufw`，且相关工具日志无失败 |
| 实体烧录成功 | 无离线/错误信号、设备确认完成下载、串口运行的是新固件 |

## 已验证基线

Code::Blocks 20.03 rev 11983、杰理工具链 2.5.0、AC79 SDK `release/AC79NN_SDK_V1.2.0` 的 `demo_hello` 已完成无头 `Release` 编译。无硬件测试中下载器返回 `Device Offline/-11`，虽生成固件且批处理退出 0，实体 Flash 仍判定未成功。
