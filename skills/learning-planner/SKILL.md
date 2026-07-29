---
name: learning-planner
description: Produce a phased, evidence-based learning plan for a technology/skill or an existing project/codebase. Works through iterative Socratic questioning and correction to diagnose the learner's current level, goal, and constraints, then outputs staged roadmaps where each stage covers objectives, deliberate-practice tasks, real projects, self-checks, and exit criteria, and revises them as the learner progresses. Methods draw on the Dreyfus skill-acquisition stages, deliberate practice, the 20-hour rule, Bloom's taxonomy, project-based learning, and spaced repetition. Use when the user wants to 学习/上手/掌握 a technology or a project and asks for a 学习方案 / 学习路线 / 学习计划 / 怎么学 / 怎么上手 / study plan / learning roadmap. It does not produce a software 设计方案/技术方案 (that is a separate software-design task and is out of scope here).
---

# Learning Planner（分阶段学习方案）

通过**不断提问 + 不断修正**摸清学习者的起点与目标，产出一份**分阶段、可动手、可自检**的学习方案，并随进度滚动修订。适用两类对象：**一门技术/技能**（如 PostgreSQL、Rust）或**一个具体项目/代码库**。

方法论综合业界公认做法：**Dreyfus 五阶段**（定阶段）、**刻意练习**（练习设计）、**20 小时法则**（拆解+快速起步）、**Bloom 认知层级**（阶段目标与自检）、**项目驱动学习**（每阶段真实产出）、**间隔复习**（留存）。

## 范围边界（学习方案，不是设计方案）

- 想**学会/上手** → 本技能，产出**学习路线**。
- 想**设计/改造**项目 → 那是**软件设计类任务**，不在本技能范围（交给设计/技术方案类流程）。
- 学"一个项目"时，可沿用通用的**代码库分析法**（自顶向下 + 自底向上）理解结构，但产出是"人怎么学会它"，不是"怎么建它"。

## 工作流

```
- [ ] 0. 评审：确认是"学习"诉求（出学习方案），不是设计/技术方案
- [ ] 1. 访谈分诊：迭代提问摸清 对象/现有水平/目标/时间/偏好/约束（单次一问，允许修正）
- [ ] 2. 拆解：把技能拆成子技能，或把项目拆成模块/关键路径
- [ ] 3. 定阶段：用 Dreyfus 阶段 + Bloom 目标动词，给每阶段定目标/内容/练习/项目/自检/出阶段标准
- [ ] 4. 输出学习方案：分阶段路线（练习 + 项目 + 间隔复习）
- [ ] 5. 迭代修正：随进度与反馈滚动更新
```

### 1. 访谈分诊（核心：提问 + 修正）

**一次只问一个关键问题**，拿到答复后再问下一个；对模糊/矛盾之处追问澄清、随时修正前面的假设。要问清（能推断的不重复问）：

- **学什么**：一门技术/技能，还是某个项目/代码库？具体是什么？
- **现有水平**：对照 Dreyfus——完全新手 / 会一点 / 能独立做 / 想精通？
- **目标**：达到哪个阶段？有没有**具体产出目标**（做出某东西 / 通过面试 / 能维护某项目）？
- **时间预算**：每周几小时、总周期多久？
- **偏好与约束**：偏看/偏做、语言、已有基础可迁移的部分。

> 访谈纪律见 [references/plan-template.md](references/plan-template.md) 的问题库；别一次抛一堆问题。

### 2. 拆解

- **技术/技能**：按 20 小时法则**解构**成子技能，砍掉"想学但这次不必"的部分（定非目标）。
- **项目/代码库**：拆成"跑起来 → 关键数据流 → 各模块 → 能改动的点"，识别**任务驱动的切入点**。

### 3–4. 定阶段并输出方案

用 Dreyfus 阶段做骨架，每阶段用 Bloom 动词写**可检验的目标**，并配**刻意练习 + 真实项目 + 间隔复习**。输出模板（技术版 / 项目版）见 [references/plan-template.md](references/plan-template.md)。每阶段必须有：

- 目标（Bloom 动词，如"能*应用*索引优化一条慢查询"）
- 学习内容（最小必要集）
- 刻意练习（有明确目标 + 反馈方式）
- 阶段项目（动手产出物）
- 自检 / 出阶段标准（达到才进下一阶段）

### 5. 迭代修正

学习者反馈"太难/太易/时间不够/方向变了" → **调整方案**（增减内容、改节奏、换项目）。学项目时，新学到的业务规则/坑建议记录到项目的**领域记忆文件**（若装有记忆类技能，交给它维护）。

## 关键原则

- **动手 > 消费**：每阶段都要有真实产出，别堆一串教程链接。
- **刻意练习**：练习要有明确目标和反馈，不是重复劳动。
- **阶段可检验**：每阶段有出阶段标准，避免"学了但不会用"。
- **深度匹配目标**：只想"够用"就别排到专家阶段（避免过度工程）。
- **先问再排**：信息不足时继续访谈，不要凭空给一份通用计划。

## 参考

- [references/learning-methods.md](references/learning-methods.md) — 各方法论详解与应用方式（Dreyfus / 刻意练习 / 20 小时 / Bloom / 项目驱动 / 间隔复习 / 代码库上手）
- [references/plan-template.md](references/plan-template.md) — 访谈问题库 + 分阶段方案模板（技术版 / 项目版）
