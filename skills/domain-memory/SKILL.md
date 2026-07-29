---
name: domain-memory
description: Maintain persistent, structured domain/business knowledge for a project across conversations — read it before writing business logic, and update it when the user states or corrects a business rule, term, or system-integration fact. Entries are confidence-scored and provenance-tracked, stored in the project's .memory/ (design borrows the popular Memory Bank layout + semantic-memory separation + Zep-style provenance/tombstones). Use when the user says 初始化领域知识 / 建记忆库 / 记住这个业务规则, before writing business logic, or when the conversation mentions domain concepts, business rules, or upstream/downstream systems.
---

# Domain Memory（项目领域知识记忆）

维护项目的**业务领域知识**——代码里读不出的"为什么 / 业务上是什么意思"，跨对话持续积累、自我修正，让 AI 写业务代码时不犯业务错误。

设计综合业界热门做法：Cursor 社区 **Memory Bank**（结构化记忆文件 + 读前/写后协议）、**语义记忆**（抽取的结构化事实，与聊天日志分离）、Anthropic **CLAUDE.md 精简索引**原则、以及 Zep/审计型记忆的**可信度 + 出处 + 墓碑**。

## 收录红线（只存"语义记忆"）

能从代码推导的一律不收：调用关系、字段类型、接口签名、注释里已写明的取值含义。**只收"资深工程师读完全部代码后仍要问人"的业务语义。** 写任何条目前做一次「信息差测试」：该内容在代码锚点（注释/类型/命名）处是否已明文可读？是 → 拒收。

## 存储结构

```
.memory/
  INDEX.md                 # 小而稳的索引（系统定位 / 术语 / 领域文件清单）——每次任务前必读
  domains/
    <域>.md                # 按业务域切分的知识文件（如 订单.md、结算.md）
```

`INDEX.md` 保持精简（借鉴 Anthropic：always-load 的内容越少越好，塞满反而被忽略）。领域文件按需加载。

## 核心协议（何时读、何时写）

- **读**：写业务代码前，先读 `INDEX.md`；任务命中某领域的关键词/目录 → 再读对应 `domains/<域>.md`。
- **用**：可信度 **1 分**的条目先问用户再用（数值类照写但插 `TODO(业务待确认)` 注释）；2 分以上可直接用；5 分冲突时以它为准。
- **写**：对话中用户**陈述 / 纠正**业务规则、术语含义、上下游对接关系时 → 抽取并写入条目（带可信度 + 出处 + 日期）。
- **维护**：新旧冲突 → 新条目胜出，旧条目**落墓碑**（保留 ID，标 superseded）；用户**重复确认** → 升分。

## 工作流

```
- [ ] 1. 定位/初始化 .memory/（无则建 INDEX.md + domains/）
- [ ] 2. 读：任务前按 INDEX 命中读对应领域文件
- [ ] 3. 用：低分先问、数值类先问，再写代码
- [ ] 4. 写：把对话中新出现的业务知识抽取成条目
- [ ] 5. 维护：冲突落墓碑、重复确认升分
```

## 可信度语义（1–5）

| 分 | 含义 | 使用方式 |
|----|------|----------|
| 1 | 推断/猜测 | 用前先问；数值类照写并插 TODO 注释 |
| 2 | 用户随口提及 | 可直接用 |
| 3–4 | 用户明确说明 | 放心用 |
| 5 | 反复确认/权威来源 | 冲突时以它为准 |

条目格式、INDEX 模板、墓碑写法等细节见 [references/memory-protocol.md](references/memory-protocol.md)。

## 关键原则

- **结构化事实，不是聊天记录**：存抽取后的业务事实，不要把对话原文倒进去。
- **溯源可审计**：每条带出处（对话 / 文档 / 代码位置）与日期，便于日后核验。
- **索引小、细节按需**：INDEX 只放定位与清单；知识正文进领域文件。
- **不确定就问，不臆造**：拿不准的标注为待确认，别用 1 分知识默默写死。

## 参考

- [references/memory-protocol.md](references/memory-protocol.md) — 条目格式、INDEX 模板、可信度/升分/墓碑规则、完整读写协议
