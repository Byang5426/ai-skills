---
name: skill-builder
description: Generate standard, spec-compliant Cursor Agent Skills from a plain-language requirement. Produces the skill directory, SKILL.md with correct frontmatter, and optional reference/example/script files. Use when the user wants to create, author, scaffold, or write a new skill, asks about SKILL.md structure, or says things like "帮我做/写/生成一个 skill / 技能".
---

# Skill Builder（生成技能的技能）

用户用自然语言描述需求，你据此产出一个**符合 Cursor 官方规范**的技能。目标：用户只描述“想要什么”，你就能一次性生成可直接使用的标准 `SKILL.md`（及需要的辅助文件）。

## 工作流（按序执行）

复制并跟踪这个清单：

```
- [ ] 0. 评审（判断该不该做成 skill；陌生场景才联网参考）
- [ ] 1. 收集需求（缺关键项才问，能推断就推断）
- [ ] 2. 设计（定 name / description / 目录结构）
- [ ] 3. 生成文件（SKILL.md + 可选 reference/examples/scripts）
- [ ] 4. 自检（对照 references/checklist.md）
- [ ] 5. 汇报（列出生成了什么、放在哪、如何触发）
```

### 0. 评审（先判断，再动手）

在生成任何文件之前，先过这道轻量决策门。

**a. 适合性判断：这个需求该做成 skill 吗？** 对照下表，不适合就直接建议正确机制并停下，不要为建而建：

| 需求特征 | 更合适的机制 |
|----------|--------------|
| 可复用的工作流 / 领域知识 / 输出格式 | ✅ **skill**（继续） |
| 需独立上下文、扮演某角色、可被派活 | **subagent** |
| 全局约束 / 编码风格 | **rule** |
| 固定触发的一段指令 | **command** |
| 连外部系统 / API / 数据库 | **MCP** |
| 一次性、不会复用 | **直接做，别建** |

**b.（可选）联网参考已有实现——仅在需要时触发。** 满足以下任一才联网：场景陌生 / 领域专业性强 / 用户明确要求参考。标准场景（commit message、代码审查等）直接走官方规范，不要联网。详细触发条件与检索方法见 [references/review-and-research.md](references/review-and-research.md)。

> 保持轻量：评审只花几句话得出结论，不要长篇论证；联网默认关闭，按需开启。

### 1. 收集需求

需要弄清 5 件事。**用户已说清或能从上下文合理推断的，不要重复问**；只对确实缺失的关键项提问（优先用 AskQuestion）：

1. **用途**：这个技能解决什么任务？
2. **位置**：个人 `~/.cursor/skills/`（跨项目）还是项目 `.cursor/skills/`（随仓库共享）？
3. **触发场景**：什么时候该自动用它？（收集触发词）
4. **领域知识**：Agent 本身不会、必须写进去的专门信息（格式、规则、约束）。
5. **输出格式**：是否有固定模板/风格。

> 用户若给了**逐字文案**，原样使用，不要改写或加戏。

### 2. 设计

- `name`：小写字母/数字/连字符，≤64 字符，语义明确（如 `processing-pdfs`，禁用 `helper`/`utils`/`tools`）。
- `description`：**第三人称**，写清 **WHAT + WHEN**，带触发词，≤1024 字符。这是最关键的字段（决定何时被调用）。
- 是否自动触发：需要“靠上下文自动触发”→ **省略** `disable-model-invocation`；只想被显式点名调用 → 设 `disable-model-invocation: true`。
- 判断是否需要拆出 `reference.md` / `examples.md` / `scripts/`（正文超长或有大量细节时拆）。

### 3. 生成文件

用下面的模板生成 `SKILL.md`。正文控制在 **500 行以内**，简洁、术语统一、示例具体、无时间敏感信息、用正斜杠路径。

```markdown
---
name: <skill-name>
description: <第三人称，WHAT + WHEN + 触发词>
---

# <Skill Title>

## Quick Start / Instructions
<清晰的分步骤指令>

## Examples
<具体示例，非抽象描述>

## Additional Resources
- 详细规则见 [reference.md](reference.md)
```

按“自由度匹配”选表达方式：多解法→文字指引；有偏好模式→模板/伪代码；操作脆弱须一致→固定脚本。

需要“需求→成品”的完整范本时，参考 [references/example-skill.md](references/example-skill.md)。

### 4. 自检

对照 [references/checklist.md](references/checklist.md) 逐项核对后再交付。

### 5. 汇报

告诉用户：生成了哪些文件、放在什么路径、`name`/`description` 是什么、会在什么场景触发、以及是否需要重启会话生效。

## 关键规则速记（详见 references/authoring-rules.md）

- description 用第三人称、含 WHAT+WHEN 和触发词——**这是技能能否被正确调用的核心**。
- SKILL.md ≤ 500 行；细节走渐进式披露，引用只保持一层深度。
- 不放 Windows 路径、不给一堆并列选项、不写会过期的时间敏感信息、术语全程统一。
- 绝不写入 `~/.cursor/skills-cursor/`（Cursor 内置保留目录）。

## 参考

- [references/review-and-research.md](references/review-and-research.md) — 适合性判断标准 + 联网参考的触发条件与方法
- [references/authoring-rules.md](references/authoring-rules.md) — 完整的写作原则、模式与反模式
- [references/example-skill.md](references/example-skill.md) — 端到端完整范例（需求→成品技能）
- [references/checklist.md](references/checklist.md) — 定稿前对照清单
