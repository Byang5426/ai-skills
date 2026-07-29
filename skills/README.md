# Skills

本目录存放各个 Agent Skill，每个技能一个子目录，目录内必须有 `SKILL.md`。

## 目录约定

```
skills/
└── <skill-name>/
    ├── SKILL.md            # 必须：主指令 + frontmatter(name/description)
    └── references/         # 可选：渐进式披露的详细文档
        └── *.md
```

## 现有技能

| 技能 | 作用 |
|------|------|
| [`skill-builder`](skill-builder/SKILL.md) | 从自然语言需求生成符合 Cursor 规范的标准技能 |
| [`project-design-planner`](project-design-planner/SKILL.md) | 分析项目并产出结构化设计方案/思路（Google Design Doc / RFC / C4 / ADR 结构） |
| [`domain-memory`](domain-memory/SKILL.md) | 跨对话维护项目业务领域知识（可信度/出处/墓碑；Memory Bank + 语义记忆 + Zep 溯源） |
| [`learning-planner`](learning-planner/SKILL.md) | 迭代提问 → 分阶段学习方案（Dreyfus / 刻意练习 / 20 小时法则 / Bloom / 项目驱动） |

## 新增技能

推荐直接用 `skill-builder`：向 AI 描述需求，它会按规范生成新技能目录。
手写时请遵循 [`skill-builder/references/authoring-rules.md`](skill-builder/references/authoring-rules.md) 与
[`skill-builder/references/checklist.md`](skill-builder/references/checklist.md)。
