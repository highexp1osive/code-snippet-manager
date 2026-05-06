# Code Snippet Manager

一个简单的claude code skills：项目过程中保存功能代码片段，供其他项目复用，减少与agent的沟通成本，提高工作效率。

## 功能

- **保存**：说"保存这段代码为 XXX"，Agent 自动存入片段库
- **检索**：说"我要 XXX 功能"，Agent 搜索、适配当前项目、补全代码
- **浏览**：说"我有哪些片段"，列出所有片段及描述
- **删除**：说"删除 XXX 片段"

## 安装

```bash
# 克隆到 Claude Code skills 目录
mkdir -p ~/.claude/skills
git clone https://github.com/highexp1osive/code-snippet-manager.git ~/.claude/skills/code-snippet-manager
```

然后在 `CLAUDE.md` 中添加：

```markdown
## 代码片段库

使用 code-snippet-manager skill 管理个人代码片段。触发词：保存这段代码为 / 我要XX功能 / 列出我的片段 / 删除XX片段。
```

或者直接：claude帮我安装https://github.com/highexp1osive/code-snippet-manager的skill。

## 使用示例

```
你：保存这段代码为 quick-sort
Agent：已保存到 ~/.claude/snippets/quick-sort.md

你：我要 quick-sort 功能
Agent：（搜索片段库 → 找到 → 按当前项目语言/风格适配 → 补全）
```

## 片段格式

每个片段是一个 `.md` 文件，YAML frontmatter + 代码体：

```markdown
---
name: 片段名称
description: 一句话描述
tags: [标签1, 标签2]
language: 语言
created: YYYY-MM-DD
---

代码内容
```

## 目录结构

```
~/.claude/
├── skills/
│   └── code-snippet-manager/
│       └── SKILL.md          ← 本技能
└── snippets/                  ← 你的片段库（数据目录）
    ├── command-parser.md
    └── ...
```

## 许可证

MIT
