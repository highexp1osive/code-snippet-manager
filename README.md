# Code Snippet Manager

Claude Code skill：双模代码管理——代码片段存取 + 收工回顾效果浓缩。

核心价值：把项目中值得复用的代码/效果提炼出来，下次跟 Agent 说半句话它就知道你想做什么，**跨语言跨平台还原同样的行为**。

## 两套模式

### 代码片段（存代码）

代码级别的复用。写了通用工具函数、非标准 API 调用模式、复杂算法之后，存起来下次直接用。

| 操作 | 说 |
|------|-----|
| 保存 | "保存这段代码为 XXX" |
| 检索 | "我要 XXX 功能" / "用之前那个 XXX" |
| 浏览 | "我有哪些片段" / "列出片段" |
| 删除 | "删除 XXX 片段" |

### 效果浓缩（存规则）★ 新增

**跨语言跨平台的行为复现**。不存代码，只存"实现说明书"：

- 状态机（所有可见状态 + 触发条件）
- 关键行为（每个阶段的输入→输出规则）
- 参数（所有可调数值和比例关系）
- 禁止项（换了平台容易掉的坑）

触发词："收工" / "下班" / "今日收尾" / "项目收尾" / "结束项目" / "回顾代码"

Agent 会扫描项目改动，识别有跨场景价值的交互效果/算法/数据模式，生成候选列表让你勾选，确认后生成浓缩片段。

### 为什么需要效果浓缩

描述"我想要那种全页滑动切换的效果"需要说 200 字。但如果片段库里已经存了这个效果的行为规则，你只需要说"用全页滑动切换那个效果"——Agent 就能在 Electron、Qt、Flutter、Unity 等任何平台还原出来。

## 安装

```bash
mkdir -p ~/.claude/skills
git clone https://github.com/highexp1osive/code-snippet-manager.git ~/.claude/skills/code-snippet-manager
```

或直接：claude 帮我安装 https://github.com/highexp1osive/code-snippet-manager 的 skill。

## 使用示例

### 代码片段

```
你：保存这段代码为 quick-sort
Agent：已保存到 ~/.claude/snippets/quick-sort.md

你：我要 quick-sort 功能
Agent：（搜索 → 找到 → 按当前项目语言/风格适配 → 补全）
```

### 效果浓缩

```
你：下班
Agent（扫描今天改动）：
  📋 本次可浓缩的代码模块：
  | # | 模块 | 效果要点 | 来源 |
  |------|---------|--------|------|
  | 1 | 拖拽排序 | 原地占位 + 跟随指针 + 插入动画 | DragList.tsx |
  | 2 | 虚拟滚动 | 仅渲染可视区 ± 缓冲区 | VirtualList.tsx |
  回复编号选择。

你：1,2
Agent：已生成 drag-sort.md + virtual-scroll.md
```

## 片段格式

### 代码片段（type: code）

```markdown
---
name: 片段名称
description: 一句话描述
tags: [标签]
language: 语言
type: code
created: YYYY-MM-DD
---

代码内容
```

### 效果浓缩（type: effect）

```markdown
---
name: 全页滑动切换
essence: 多面板横向滑动，当前面板居中吸附，其余自动对齐边缘
key_techniques: [三阶段状态机, 惯性跟踪, 阈值吸附, rubber-band 回弹]
type: effect
created: 2026-05-07
---

## 状态机
idle → tracking (pointerdown)
tracking → settling (pointerup)
settling → idle (动画结束)

## 关键行为
跟踪阶段: 面板位置 = 基准偏移 + 本次位移
沉降阶段: 位移 > 阈值 → 切到相邻面板，否则回弹

## 参数
面板宽 = 容器宽 × 0.85，间距 = 16px
切换阈值 = 面板宽 × 0.3

## 禁止项
- tracking 中禁止沉降动画
- settling 中忽略所有 pointer 事件
```

## 目录结构

```
~/.claude/
├── skills/
│   └── code-snippet-manager/
│       └── SKILL.md          ← 本技能
└── snippets/                  ← 片段库（数据目录）
    ├── command-parser.md      ← 代码片段
    ├── 全页滑动切换.md        ← 效果浓缩
    └── ...
```

## 关联仓库

- [My Code Snippets](https://github.com/highexp1osive/my-code-snippets) — 个人代码片段收集

## 许可证

MIT
