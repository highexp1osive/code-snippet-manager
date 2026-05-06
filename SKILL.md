---
name: code-snippet-manager
description: Use when the user asks to save code as a named snippet, retrieve saved code snippets by name or description, browse their personal snippet library, or delete saved snippets. Triggers on phrases like "保存这段代码为", "收藏到片段库", "记住这个效果", "我要XX功能", "调出XX的代码", "用之前那个XX", "我有哪些片段", "列出片段", "删除XX片段", "save this as", "snippet this", "I need the XX function".
---

# 代码片段管理

## 概述

`~/.claude/snippets/` 目录作为个人代码片段库。用户用自然语言存取，Agent 负责存储、搜索、适配和补全。零依赖，纯文件系统。

## 片段格式

每个片段一个 `.md` 文件，YAML frontmatter + 代码体：

```
---
name: {用户命名}
description: {一句话：做什么、适用场景}
tags: [{标签}]
language: {语言}
created: {YYYY-MM-DD}
---

{代码}
```

## 保存

触发：**保存这段代码为 / 记住这个 / 收藏 / save as / snippet this**

1. 识别对话中最近编写的代码
2. 创建 `~/.claude/snippets/{name}.md`，用上述格式
3. 去掉项目特定硬编码（路径、密钥、配置值）
4. 同名片段先询问是否覆盖

## 检索

触发：**我要XX功能 / 调出XX / 用之前的XX / I need XX**

1. `ls ~/.claude/snippets/` 列出所有片段（优先用 Bash，Windows 上 Glob 展开 `~` 容易超时）
2. 按文件名、name、description、tags 模糊匹配
3. 读最匹配的文件
4. **适配当前项目后补全**（语言语法、导入路径、命名风格、框架 API）

## 浏览与删除

浏览：**我有哪些片段 / 列出片段 / list snippets** → 读所有 frontmatter，输出 name + description + tags
删除：**删除XX片段 / delete snippet XX** → 删除对应文件

## 红线

| 禁止 | 必须 |
|------|------|
| 检索后原样粘贴 | 根据当前项目语言/框架/风格适配 |
| 保存硬编码的项目特定值 | 只保留通用逻辑 |
| 不读文件就猜测内容 | 先读文件确认 |
