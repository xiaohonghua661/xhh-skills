# xhh-skills —— 跨项目 skills 母本库

本仓库是个人 skill 的**母本（source of truth）**，按**领域分类**存放。项目里的 skill 是**子本**，通过分支 `yueting` 订阅本库更新。

## 约定

- **母本分支**：`yueting`。子本比对该分支 HEAD 是否变化决定是否同步。
- **子本同步**：子本每天最多检查一次；`yueting` 有变化时**询问用户**再同步，不自动改（见 dd `#SKILLS`「母本订阅同步」）。
- **分类**：每个领域一个目录（中文名），领域内每个 skill 一个子目录，含 `SKILL.md`。
- **版本**：frontmatter 必含 `name` / `description` / `version`；每次实质修改 `version` 自增。

## 分类与 skill

| 分类 | 目录 | skill | 用途 |
|---|---|---|---|
| 嵌入式 | `嵌入式/` | code-comment-style | 约束代码注释写法（文件头 / 函数说明块 / 步骤 / 行内为什么 / 溯源 / 中文 UTF-8） |
