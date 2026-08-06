# xhh-skills —— 跨项目 / 跨 CLI skills 母本库

本仓库是个人 skill 的**唯一真相源（source of truth）**，按领域分类存放。本机的
Codex 与 dd 软链接（junction）回本库，**改这里即时生效**；Claude Code 那一端由
cc-switch 同步（是副本、有延迟），详见下面「本机如何接入」。

## 约定

- **母本分支**：`yueting`。
- **更新策略写在每个 skill 自身**：`SKILL.md` frontmatter 带 `version` / `upstream` / `branch` / `path`。
- **单一真相源**：同一能力只定义一处；相关 skill 用引用而非复制（如 STM32 skill 的注释部分引用 `code-comment-style`）。

## 分类与 skill

| 分类 | 目录 | skill | 用途 |
|---|---|---|---|
| 嵌入式 | `嵌入式/code-comment-style/` | code-comment-style | 代码注释写法约束——**注释规范唯一真相源** |
| 嵌入式 | `嵌入式/stm32-xzzs-xg/` | stm32-xzzs-xg | STM32 编码 / 协作组织习惯；注释引用 code-comment-style |
| 嵌入式 | `嵌入式/stm32-tqyz-qtgc-dqdm/` | stm32-tqyz-qtgc-dqdm | STM32 传感器驱动提取 / 移植；注释引用 code-comment-style |
| 嵌入式 | `嵌入式/jieli-headless-build/` | jieli-headless-build | 杰理 AC79 Code::Blocks 无头编译、打包与烧录成功判据 |
| 学习方法 | `学习方法/xx/` | xx | 学习闭环（主动回忆+间隔重复+费曼+交错锚定+上手），触发 `/xx` |

## 本机如何接入（单一源，两种分发）

真身只在本库。三端形态**不对称**，别按一种理解：

| 端 | 形态 | 改本库后 |
|---|---|---|
| Codex | `~/.codex/skills/<名>` ─junction→ `本库/<分类>/<名>` | ✅ 即时生效 |
| dd | `DD/data/skills/xhh-skills` ─junction→ 本库根目录（扫描器跟随 junction） | ✅ 即时生效 |
| Claude Code | `~/.claude/skills/<名>` 由 **cc-switch 同步**，是副本不是链接 | ⏳ 等下次同步 |

Claude 那端为什么不是 junction：Windows 未开发者模式时建符号链接报 `os error 1314`，cc-switch
回落为文件复制；而且 `~/.claude/skills` 归 cc-switch 管——**手工建的 junction 会被下次同步覆盖，
别建**。要让 Claude 立刻拿到新版，走 cc-switch 重新同步，不要绕过它动目录。

建链（仅 Codex / dd 两端）：`mklink /J`（Windows，免管理员）；解链只用 `rmdir`，**禁 `rm -rf`**。

## 如何发布更新

- dd 工作流 **`%git发布`**：commit → push `yueting` → 打新 tag 存源码快照 → Releases 发 changelog。
- 发一个 skill 用 **`%发布skill`**（写母本 + 校验三端 junction + 调 `%git发布` + 验收 dd 即时读取）。
- 不手工操作本库 git。
