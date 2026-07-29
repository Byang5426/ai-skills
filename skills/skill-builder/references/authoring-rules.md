# 技能写作规则（完整版）

供 `skill-builder` 在生成技能时参考。基于 Cursor 官方 `create-skill` 规范。

## 目录结构

```
skill-name/
├── SKILL.md          # 必须
├── reference.md      # 可选：详细文档
├── examples.md       # 可选：示例
└── scripts/          # 可选：工具脚本
```

存放位置：

| 类型 | 路径 | 范围 |
|------|------|------|
| 个人 | `~/.cursor/skills/skill-name/` | 所有项目 |
| 项目 | `.cursor/skills/skill-name/` | 随仓库共享 |

**禁止**写入 `~/.cursor/skills-cursor/`（Cursor 内置技能保留目录，系统自动管理）。

## Frontmatter 字段

| 字段 | 要求 | 作用 |
|------|------|------|
| `name` | 必填，≤64 字符，小写字母/数字/连字符 | 唯一标识 |
| `description` | 必填，≤1024 字符，非空 | 决定何时调用 |
| `disable-model-invocation` | 可选 | `true`=仅显式点名加载；省略=可按上下文自动触发 |

## 写好 description（最关键）

1. **第三人称**：Agent 会把它注入系统提示词。
   - ✅ "Processes Excel files and generates reports"
   - ❌ "I can help you..." / "You can use this to..."
2. **WHAT + WHEN 都要写**，并带触发词。
   - ✅ "Extract text and tables from PDF files, fill forms, merge documents. Use when working with PDF files or when the user mentions PDFs, forms, or document extraction."
   - ❌ "Helps with documents"
3. 若用户会用中文/英文两种说法触发，触发词两种都写进去。
4. **YAML 安全**：`description` 不加引号时，值里**不能出现"冒号+空格"（`: `）**，否则会被当成嵌套映射报错（`mapping values are not allowed`）。改写成破折号/逗号，或整体用引号包裹（但要避开值里同款引号）。同理别以 `#`、`[`、`{`、`&`、`*` 等特殊字符开头。

## 核心写作原则

1. **简洁至上**：Agent 已经很聪明，只补它不知道的。每段都要值回 token。
2. **SKILL.md ≤ 500 行**。
3. **渐进式披露**：核心放 SKILL.md，细节拆到独立文件按需读；**引用只保持一层深度**（深层嵌套可能被部分读取）。
4. **自由度匹配任务脆弱性**：
   - 高（文字指引）：多种合法做法、依赖上下文，如代码审查
   - 中（模板/伪代码）：有偏好模式但允许变化，如报告生成
   - 低（固定脚本）：操作脆弱、必须一致，如数据库迁移

## 常用模式

- **模板模式**：给出固定的输出格式模板。
- **示例模式**：输出质量依赖示例时，给 2+ 个具体 input→output。
- **工作流模式**：复杂操作拆成带勾选框的步骤清单。
- **条件工作流**：在决策点分支引导（“创建新内容 → 走 A；编辑已有 → 走 B”）。
- **反馈闭环**：质量关键任务加“执行→校验→失败则修→通过才继续”。

## 工具脚本（可选）

预置脚本优于让 Agent 现写：更可靠、省 token、省时、结果一致。要说明脚本是**执行**（最常见）还是**当参考读取**，并注明所需依赖。

## 反模式（务必避免）

1. **Windows 路径**：用 `scripts/helper.py`，不用 `scripts\helper.py`。
2. **一堆并列选项**：给一个默认 + 例外出口，别罗列 “A 或 B 或 C 或…”。
3. **时间敏感信息**：不写 “2025年8月前用旧API”；改用「Current method」+ 折叠的「Old patterns」。
4. **术语不统一**：选定一个词贯穿（始终 “field”，不要混用 box/element/control）。
5. **含糊技能名**：禁用 `helper`/`utils`/`tools`，要 `processing-pdfs` 这类。

## 生成流程（内部四阶段）

1. **发现**：用途、位置、触发场景、约束、可参考的既有范例。
2. **设计**：起名、写 description、列出正文小节、判断是否需要辅助文件/脚本。
3. **实现**：建目录 → 写 SKILL.md → 写参考文件 → 写脚本（如需要）。
4. **验证**：见 checklist.md。
