# 定稿前对照清单

交付生成的技能前，逐项核对：

## 核心质量
- [ ] description 具体、含关键触发词
- [ ] description 同时写清 WHAT 与 WHEN
- [ ] description 用第三人称
- [ ] frontmatter 是合法 YAML：不加引号的 `description` 值里无"冒号+空格"（`: `），不以特殊字符开头
- [ ] SKILL.md 正文 < 500 行
- [ ] 全程术语统一
- [ ] 示例具体，非抽象

## 结构
- [ ] 文件引用只保持一层深度
- [ ] 恰当使用渐进式披露
- [ ] 工作流步骤清晰
- [ ] 无时间敏感信息
- [ ] `name` 语义明确（非 helper/utils/tools）
- [ ] 未写入 `~/.cursor/skills-cursor/`
- [ ] 自包含：正文未硬写依赖其它技能的名字（跨技能边界放 description、用能力描述）

## 若包含脚本
- [ ] 脚本真正解决问题，而非敷衍
- [ ] 所需依赖已注明
- [ ] 错误处理明确、有帮助
- [ ] 无 Windows 风格路径
