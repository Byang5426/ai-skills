# ai-skills

> 个人 AI 助手能力库 —— 沉淀可复用的 **skills（技能）**、**rules（规则）** 与 **hooks（钩子）**，
> 适配 Cursor、Claude Code 等支持 Agent Skills 的 AI 工具。

一套配置，跨项目复用：把常用的工作流、领域约定和自动化固化成标准文件，让 AI 每次都按你的规范干活，而不是每次从零解释。

---

## 目录结构

```
ai-skills/
├── skills/                 # Agent 技能（每个技能一个目录，内含 SKILL.md）
│   └── skill-builder/      # 从需求生成标准技能的“元技能”
├── rules/                  # 可复用规则（编码规范、角色设定等）
├── hooks/                  # 事件钩子脚本（会话启动、提交前等）
├── LICENSE
└── README.md
```

设计原则：**技能自包含**（每个技能目录独立、可单独拷用）、**渐进式披露**（核心放 `SKILL.md`，细节拆到 `references/`）、**工具中性**（尽量不绑死单一 AI 工具）。

---

## 技能一览

| 技能 | 作用 | 文档 |
|------|------|------|
| **skill-builder** | 用自然语言描述需求，自动生成符合 Cursor 官方规范的标准技能（含目录、`SKILL.md`、参考文件） | [SKILL.md](skills/skill-builder/SKILL.md) |
| **project-design-planner** | 分析一个项目（现有代码库或描述中的项目），产出结构化设计方案/思路（背景、目标、设计、备选取舍、横切、落地），结构综合自 Google Design Doc / RFC / C4 / ADR | [SKILL.md](skills/project-design-planner/SKILL.md) |
| **domain-memory** | 跨对话维护项目的业务领域知识（带可信度/出处/墓碑），写业务代码前读、学到新规则时写；综合 Memory Bank / 语义记忆 / Zep 溯源等热门做法 | [SKILL.md](skills/domain-memory/SKILL.md) |

> 更多技能持续添加中。

---

## 安装 / 使用

这些技能遵循 Cursor 的 [Agent Skills](https://docs.cursor.com) 规范：一个技能 = 一个含 `SKILL.md` 的目录。

### 方式一：安装单个技能（推荐）

把需要的技能目录拷到你的个人技能目录：

```bash
# Cursor 个人技能目录（跨所有项目可用）
git clone https://github.com/Byang5426/ai-skills.git ~/ai-skills
cp -R ~/ai-skills/skills/skill-builder ~/.cursor/skills/
```

重启 / 重新加载会话后即可使用。

### 方式二：软链接（跟随仓库自动更新）

```bash
git clone https://github.com/Byang5426/ai-skills.git ~/ai-skills
ln -s ~/ai-skills/skills/skill-builder ~/.cursor/skills/skill-builder
# 之后 git pull 即同步最新
```

### 项目级安装

若只想在某个项目里用，拷到该项目的 `.cursor/skills/` 即可（会随仓库共享给协作者）。

> 存放位置说明：`~/.cursor/skills/` 为个人（跨项目），`<项目>/.cursor/skills/` 为项目级。
> 切勿放进 `~/.cursor/skills-cursor/`（Cursor 内置技能保留目录）。

---

## 外部依赖

本仓库的技能是**说明书**，不打包第三方工具本体。以下为可选的外部能力，按需在每台机器单独安装（**不要**把它们的 skill 拷进本仓库）：

| 依赖 | 用途 | 获取方式 |
|------|------|----------|
| **Firecrawl** | 联网搜索 / 网页抓取（供技能里的"联网参考"等使用） | Cursor 用**官方 firecrawl 插件**；其他工具用 `firecrawl init --agent <工具>` |

> 说明：Firecrawl 的 skill 由 Cursor 插件或 `firecrawl init` 安装到**工具的全局目录**，不进本项目。它的 **CLI 本体 + 登录凭据是每台机器各自的**，不随仓库同步——见下方"新机器环境准备"。

---

## 新机器环境准备

在一台新电脑上恢复本仓库能力，做两件事：

**1. 装技能（本仓库）**

```bash
git clone https://github.com/Byang5426/ai-skills.git ~/ai-skills
cp -R ~/ai-skills/skills/* ~/.cursor/skills/     # 或按需软链单个技能
```

**2. 装联网能力（Firecrawl，可选但推荐）**

```bash
npm install -g firecrawl-cli   # 装 CLI 本体（Cursor 里首次联网也会自动装）
firecrawl login --browser      # 浏览器登录（凭据不跨机器同步，必须本人点一次）
firecrawl --status             # 确认已认证
```

> 装完重启 / 重新加载 AI 会话即可。技能本身跟着仓库走，Firecrawl 的 CLI 与登录每台机器都要单独来一次。

---

## 新增你自己的技能

最省事的方式就是用本仓库的 **`skill-builder`**：装好后，直接对 AI 说

> “帮我做一个技能：<描述你的需求>”

它会按规范收集要点并生成好目录与 `SKILL.md`。手写时请遵循：

- [authoring-rules.md](skills/skill-builder/references/authoring-rules.md) — 完整写作规则、模式与反模式
- [checklist.md](skills/skill-builder/references/checklist.md) — 定稿前对照清单
- [example-skill.md](skills/skill-builder/references/example-skill.md) — 端到端完整范例

### 技能规范速记

- `SKILL.md` 必须有 frontmatter：`name`（小写-连字符，≤64 字符）、`description`（第三人称，写清 WHAT + WHEN + 触发词，≤1024 字符）。
- 正文 ≤ 500 行；细节走渐进式披露，引用只保持一层深度。
- 示例要具体、术语统一、路径用正斜杠、不写会过期的时间敏感信息。

---

## License

[MIT](LICENSE) © 2026 Byang5426
