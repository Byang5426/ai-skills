---
name: project-design-planner
description: Analyze a project (an existing codebase or a described project) and produce a structured design proposal — background, goals/non-goals, requirements, the design with diagrams, alternatives with trade-offs, cross-cutting concerns and rollout. Use when the user asks to analyze a project and output a 设计方案 / 思路 / 技术方案 / 架构设计 / design doc, or says things like "分析这个项目给我一个设计方案 / analyze this project and propose a design".
---

# Project Design Planner（项目分析 → 设计方案）

分析一个项目（现有代码库，或用户描述中的新项目），产出一份**结构化的设计方案/思路**。方法论与输出结构综合自业界标准：Google Design Doc、RFC、C4 模型、ADR，以及代码库理解的自顶向下/自底向上法。

## 运行建议（上下文隔离）

分析较大项目会读大量文件、产生大量上下文。**优先在 subagent 里跑"分析"阶段**（如内置 Explore/Plan，或专门的分析 subagent），只把结论带回主对话，避免污染主上下文。小项目可直接进行。

## 工作流（按序执行）

```
- [ ] 1. 定界（确认分析对象、目标/非目标、产出深度）
- [ ] 2. 分析项目（自顶向下 + 自底向上，见 references/analysis-method.md）
- [ ] 3. 产出设计方案（按标准结构，见 references/design-doc-template.md）
- [ ] 4. 记录关键取舍（ADR 思路）
- [ ] 5. 交付并请评审（点出最需拍板的取舍）
```

### 1. 定界

先确认（缺关键项才问，能推断就推断）：

- **分析对象**：现有代码库，还是一个"描述中的新项目"？
- **目标与非目标**：这次设计要解决什么、明确不解决什么。
- **产出深度**：轻量"思路概述"，还是"完整设计文档"。
- **约束**：技术栈、性能/安全/合规等硬约束。

### 2. 分析项目

用组合方法（详见 [references/analysis-method.md](references/analysis-method.md)）：

- **自顶向下**：入口 → 目录/模块结构 → 依赖关系 → 关键数据流。
- **自底向上**：核心文件/关键类/热点函数往上还原设计。
- 用 **C4 视角**分层看：System Context → Container → Component。

产出「现状理解」：架构概览、模块职责、数据流、技术栈、痛点/风险。

### 3. 产出设计方案

套用标准结构（详见 [references/design-doc-template.md](references/design-doc-template.md)，源自 Google Design Doc + 三层洋葱 + RFC）：

1. **背景与范围**（客观事实 + 系统上下文图）
2. **目标与非目标**（非目标 = 本可作为目标但明确不做的）
3. **需求**（功能性 + 非功能性）
4. **设计方案**：先总览后细节；接口 / 数据存储 / 关键流程；配图（mermaid / C4）
5. **备选方案与取舍**（每个备选的 trade-off 及为何不选）
6. **横切关注点**（安全、隐私、可观测性、性能）
7. **里程碑 / 落地步骤**
8. **风险与未决问题**

> 三层洋葱原则：问题/目标/需求 → 功能规格（外部行为）→ 技术规格（内部实现），层层递进，前一层不成立则后面无意义。

### 4. 记录关键取舍（ADR）

对重要设计决策，用 ADR 三段式记录：**Context（背景/约束）→ Decision（决定）→ Consequences（后果/代价）**。可内嵌在方案里或单列。

### 5. 交付并请评审

输出方案后，**主动点出 1–3 个最需要用户拍板的取舍点**（借鉴设计评审：评审的价值在于早期发现问题、纳入横切关注点），而不是直接当成定案。

## 关键原则

- **写取舍，而非只写结论**：设计文档的长期价值在于记录"为什么这样选"。
- **图优先**：结构/流程用图说明（系统上下文图、C4、mermaid）比大段文字清晰。
- **接口/数据不要照抄**：只写与设计和取舍相关的部分，避免冗长且易过时。
- **深度匹配需求**：小需求给"思路概述"即可，别硬套完整文档模板（避免过度工程）。

## 参考

- [references/analysis-method.md](references/analysis-method.md) — 代码库/项目分析方法论（自顶向下、自底向上、C4、5 层理解）
- [references/design-doc-template.md](references/design-doc-template.md) — 完整设计方案模板 + ADR 模板 + 示例骨架
