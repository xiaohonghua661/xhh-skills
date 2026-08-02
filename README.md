# xhh-skills —— 跨项目 skills 母本库

本仓库是个人 skill 的**母本（source of truth）**，按**领域分类**存放。项目里的 skill 是**子本**，通过分支 `yueting` 订阅本库更新。

## 约定

- **母本分支**：`yueting`。
- **更新策略写在每个 skill 自身**：每份 `SKILL.md` 的 frontmatter 带 `version` / `upstream` / `branch` / `path`，自描述来源与版本。
- **子本同步**：子本每天最多检查一次；`yueting` 有变化时**询问用户**再同步，不自动改（见 dd `#SKILLS`「母本订阅同步」）。
- **分类**：每个领域一个目录（中文名），领域内每个 skill 一个子目录含 `SKILL.md`。
- **单一真相源**：同一能力只在一处定义；相关 skill 用引用而非复制（如 STM32 skill 的注释部分引用 `code-comment-style`）。

## 分类与 skill

| 分类 | 目录 | skill | 用途 |
|---|---|---|---|
| 嵌入式 | `嵌入式/code-comment-style/` | code-comment-style | 约束代码注释写法（文件头 / 函数说明块 / 步骤 / 行内为什么 / 溯源 / 中文 UTF-8）——**注释规范唯一真相源** |
| 嵌入式 | `嵌入式/stm32-xzzs-xg/` | stm32-xzzs-xg | STM32 编码与协作组织习惯（BSP / main.c 组织 / OneNet / OLED / 按键 / EIDE）；注释引用 code-comment-style |
| 嵌入式 | `嵌入式/stm32-tqyz-qtgc-dqdm/` | stm32-tqyz-qtgc-dqdm | STM32 传感器驱动提取 / 移植工作流；注释引用 code-comment-style |
