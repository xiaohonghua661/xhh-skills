# skills —— 跨项目 skills 母本仓库

本仓库是各项目 skill 的**母本（source of truth）**。项目里的 skill 是**子本**，通过分支 `yueting` 订阅本仓库更新。

## 约定

- **母本分支**：`yueting`。子本比对该分支 HEAD 是否变化决定是否同步。
- **子本同步**：子本每天最多检查一次；`yueting` 有变化时**询问用户**再同步，不自动改（详见各 CLI 的 dd `#SKILLS` 规约“母本订阅同步”）。
- 每个 skill 一个目录，含 `SKILL.md`，frontmatter 必含 `name` / `description` / `version`；每次实质修改 `version` 自增。

## 现有 skill

| 目录 | name | 用途 |
|---|---|---|
| `code-comment-style/` | code-comment-style | 约束代码注释写法（文件头/函数说明块/步骤/行内为什么/中文 UTF-8） |
