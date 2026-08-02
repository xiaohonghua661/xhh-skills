---
name: stm32-tqyz-qtgc-dqdm
description: 提取/移植 STM32 传感器驱动（MQ7/MQ2/DHT11 等）到当前工程指定路径，按模板（如 Hardware/bsp_co2）整理宏定义、端口配置、函数命名。注释写法统一见 code-comment-style。适用于“从别的工程提取传感器代码”“按 bsp_co2 重写驱动”“生成可移植 GPIO/ADC 模板”。
version: 1
upstream: https://github.com/xiaohonghua661/xhh-skills
branch: yueting
path: 嵌入式/stm32-tqyz-qtgc-dqdm/SKILL.md
---

# STM32 传感器驱动提取 / 移植

## 注释规范（统一引用，不在此重复）

本 skill 产出的中文注释**统一遵循 `code-comment-style`**（母本 xhh-skills `嵌入式/code-comment-style/`）：文件头块、函数傻瓜式说明块、行内讲“为什么”、溯源、中文 UTF-8。本文件只讲移植工作流。

## 文件读取要求

- 读取文本文件用 .NET File.ReadAllText 并显式 UTF-8，避免中文乱码。

## 目标

- 将外部工程的传感器驱动提取到当前工程指定目录。
- 按模板统一宏定义、端口配置、函数命名。

## 工作流程

### 1) 定位源代码

- 用 `rg` 在用户提供的工程路径中搜目标传感器关键字（MQ7 / MQ2 / DHT11）。
- 找不到源码就告知用户并请求具体源文件或工程目录。

### 2) 创建目标目录与文件

- 在目标工程建 `Hardware/<sensor>`（如 `Hardware/mq`），新建 `<sensor>.h` 与 `<sensor>.c`，命名与需求一致。

### 3) 按模板整理头文件

- 以用户给定模板为准（默认参考 `Hardware/bsp_co2/bsp_co2.h` 风格）。
- 宏定义集中管理，端口 / 时钟 / 通道便于修改。

### 4) 按模板整理源文件

- 保持原有功能逻辑，确保可编译。
- GPIO / ADC 初始化拆分为静态函数，增强移植性。
- 注释按 `code-comment-style` 补齐（含函数“上手友好”说明块）。

### 5) 校验与提示

- 源码依赖外部 ADC / Delay 模块时保留原依赖并说明。
- 提醒用户把新增 `.c` 文件加入 Keil / 工程分组。
- 用户提供新模板时，优先按新模板重排格式与注释风格。

## 输出要求

- 端口宏定义集中且易改，格式对齐、命名清晰。
- 不改动与传感器无关的工程逻辑，避免引入副作用。
- 注释规范一律走 `code-comment-style`，本 skill 不重复注释细则。
