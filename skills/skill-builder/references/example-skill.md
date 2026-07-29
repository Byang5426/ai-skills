# 端到端示例：从需求到成品技能

给 `skill-builder` 一个具体范本：用户一句话需求 → 收集到的要点 → 最终产出的完整技能。生成新技能时可模仿此结构与颗粒度。

## 输入（用户需求）

> “帮我做个技能：每次提交代码时，按 Angular 规范生成 commit message。”

## 收集到的要点

- **用途**：分析暂存区改动，产出符合 Angular 规范的 commit message。
- **位置**：个人 `~/.cursor/skills/`（跨项目通用）。
- **触发场景**：用户请求写/生成 commit message，或审查暂存改动时。
- **领域知识**：Angular commit 格式（type(scope): subject + body）、允许的 type 列表。
- **输出格式**：固定的 commit 文本格式。
- **自由度**：中（有固定格式，但 subject/body 内容需按 diff 生成）→ 用模板 + 示例。

## 产出目录

```
angular-commit-message/
└── SKILL.md
```

（领域知识不多，正文即可容纳，无需拆分参考文件。）

## 产出的 SKILL.md（完整）

```markdown
---
name: angular-commit-message
description: Generate Angular-convention commit messages by analyzing staged git changes. Use when the user asks to write or generate a commit message, or when reviewing staged changes for a commit.
---

# Angular Commit Message

## Instructions

1. 读取暂存改动：`git diff --staged`
2. 判定 type（见下表）与可选 scope（受影响的模块/目录）
3. 写一句祈使句 subject（≤50 字符，句末不加句号）
4. 如有必要，空一行后写 body 说明“为什么”，而非“改了什么”

## Type 列表

| type | 含义 |
|------|------|
| feat | 新功能 |
| fix | 缺陷修复 |
| docs | 文档 |
| refactor | 重构（非功能非修复） |
| perf | 性能 |
| test | 测试 |
| chore | 构建/杂务 |

## 格式模板

\`\`\`
<type>(<scope>): <subject>

<body>
\`\`\`

## Examples

输入：新增 JWT 登录接口与校验中间件
输出：
\`\`\`
feat(auth): implement JWT-based authentication

Add login endpoint and token validation middleware
\`\`\`

输入：修复报表时区导致日期显示错误
输出：
\`\`\`
fix(reports): correct date formatting in timezone conversion

Use UTC timestamps consistently across report generation
\`\`\`
```

## 交付汇报（示范）

> 已创建 `angular-commit-message`，位于 `~/.cursor/skills/angular-commit-message/SKILL.md`。
> 触发场景：请求生成 commit message 或审查暂存改动时自动生效。
> 内容不多，未拆分参考文件。可能需重启会话后识别。
