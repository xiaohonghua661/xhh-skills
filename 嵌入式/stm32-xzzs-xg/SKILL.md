---
name: stm32-xzzs-xg
description: STM32 项目协作与编码组织习惯（BSP 模板风格、main.c 组织、OneNet/AIR7804G 交互、OLED/按键习惯、EIDE 验证）。注释写法统一见 code-comment-style，本 skill 不再重复。用于按用户习惯改/新增 STM32 代码时。
version: 1
upstream: https://github.com/xiaohonghua661/xhh-skills
branch: yueting
path: 嵌入式/stm32-xzzs-xg/SKILL.md
---

# STM32 编码与协作组织习惯

## 注释规范（统一引用，不在此重复）

本 skill 的注释写法**统一遵循 `code-comment-style`**（母本 xhh-skills `嵌入式/code-comment-style/`）：文件头功能总览块、函数傻瓜式说明块、流程步骤注释、行内讲“为什么”、覆盖清单、全中文 UTF-8 无 BOM、溯源。补 / 改注释时按它执行；本文件只保留 STM32 专属的编码组织习惯。

## 文件读取要求

- 读取文本文件用 .NET File.ReadAllText 并显式 UTF-8：`[System.IO.File]::ReadAllText('path', [System.Text.Encoding]::UTF8)`，避免中文乱码。

## 使用范围

- 按用户习惯改 / 新增 STM32 代码。
- 对齐 BSP 模板风格（如 `Hardware/bsp_co2`）。

## 代码风格与结构

- 新增驱动 / 模块按 `Hardware/bsp_co2` 风格：端口宏可配置、便于移植。
- 宏命名“端口 / 引脚 / 时钟”三段式，便于替换。
- 过程式写法，避免复杂抽象。

## main.c 组织规范

### 头文件组织

- 涉及字符串格式化时顶部包含 `<string.h>`。
- 先板级 / 定时器 / 按键头文件，再功能类（OLED、ADC、SHT、USART、LoRa、Flash），功能类头文件后加简短中文说明。

### 全局变量与宏

- 常量用 `#define` + 简短中文说明；业务状态用基础类型；每个变量组注释作用。

### Init 模板

- 初始化顺序固定：`BoardInitMcu` → `BoardInitPeriph` → `keys_init` → `setTimerCallback` → `TimInit`。

### 函数风格

- 职责单一（OLED 显示 / 按键扫描 / 串口收发 / 采集 / 无线）；命名简短直观，允许拼音；实现放 main 后需在前面声明 + 说明块。

### OLED 显示与数值

- `OLED_ShowCHinese` / `OLED_ShowString` 坐标写明；`sprintf` 到 `uint8_t` 数组再显示；需对齐时给坐标计算注释。

### 按键处理

- `isKeyXPressed()` + `resetKeyX()` 消抖；光标移动先清旧位再绘新位；模式切换用 Flag + `switch` / `if`。

### 主函数入口

- main 上方简洁注释；内初始化外设后进 `while(1)` 调业务函数。

## UI / OLED 显示

- 文本不超 128x64 可显示范围，注意行距与字符数；调试信息用可开关的宏 / 注释行。

## OneNet / AIR7804G 交互习惯

- 上报 JSON 与约定格式一致、字段顺序可读。
- 指令响应复用收到的 `id` 回包：`{"id":"X","code":200,"msg":"success"}`；`id` 按要求自增。

## 用户追加习惯

- 标注式注释：就地补中文行内注释并删占位，不改逻辑；`if / else` 条件后补含义注释。
- 按键逻辑要求“放在 main 里”时不新建 app_key；已建需合并回 main，删文件前先确认。
- 编码修复：中文变“?”时用 UTF-8 无 BOM 重写；必要时 main.c 用 Unicode 转义，脚本检查是否仍含“?”或 U+FFFD。
- 本地验证：改后优先跑 EIDE unify_builder（build/Target 1/builder.params）记录结果；工具不可用需说明原因。
- 禁止擅自新增 `.c / .h`：能复用就写现有 `bsp_key` 或 main；确需新增先询问。
