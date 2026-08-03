---
name: testing
description: Write, add, and improve tests and drive changes test-first. Follows the RED-GREEN-REFACTOR TDD cycle, the testing pyramid, the AAA (Arrange-Act-Assert) structure and FIRST principles, and avoids common test anti-patterns (over-mocking, testing implementation details, flaky and assertion-roulette tests). Runs the tests as the verification loop and shows evidence. Use when the user wants to 写测试 / 加测试 / 补测试 / 测试用例 / TDD / 单元测试, set up a test for a feature or bugfix, or asks to improve test quality or coverage.
---

# Testing（写测试 / TDD）

把"写测试 → 跑测试 → 按结果迭代"固化成可复用流程。测试就是"给 Claude 一个能自己跑的验证"里的那个**验证闭环**——写完让它自己跑、按结果改，并给出证据。

## 工作流（默认 TDD：红 → 绿 → 重构）

```
- [ ] 1. 明确行为与验收：测什么行为、输入→期望输出、边界与 unhappy path
- [ ] 2. RED  先写会失败的测试：AAA 结构，覆盖 正常 / 边界 / 错误路径
- [ ] 3. GREEN 写最小实现让测试通过
- [ ] 4. REFACTOR 重构，保持测试全绿
- [ ] 5. 跑测试并给证据（贴运行结果，别只说"过了"）
```

- **修 bug**：先写一个**能复现的失败测试**，再修（复现 → 修复 → 回归防再犯）。
- **给已有代码补测试（非 TDD）**：先覆盖最有价值的（核心路径 + 已知坑），再逐步补边界；先看项目现有测试怎么写，沿用其框架与约定。

## 测试设计原则（详见 [references/test-principles.md](references/test-principles.md)）

- **测试金字塔**：多单元、适量集成、少 E2E。
- **AAA**：每个测试分 Arrange / Act / Assert；**一个测试一个 Act、断言聚焦一个行为**。
- **FIRST**：Fast / Isolated（互不依赖、不依赖执行顺序）/ Repeatable / Self-validating（自动判定通过失败）/ Timely。
- **测行为，不测实现细节**：重构不改行为时，测试不该变红。
- **覆盖 unhappy path**：错误 / 边界 / 空值 / 超时，不只测正常流程。

## 要避开的反模式（详见 [references/anti-patterns.md](references/anti-patterns.md)）

- **过度 mock**：非要 mock 才能测，往往是耦合过紧的信号。
- **测实现细节**：脆弱，一重构就红。
- **flaky（不稳定）**：硬等待(sleep)、依赖时间/时区/随机/执行顺序/共享状态。
- **assertion roulette**：一个测试塞一堆无关断言，失败了不知道哪条挂。
- **断言过弱**（只断"没抛异常"）、**快照滥用**。

## Cursor / 前端提示

- UI/浏览器测试用**稳定 test-id 或语义选择器**，别用脆弱 CSS 路径或硬等待。
- 用 **Page Object Model** 组织浏览器测试；用**工厂**生成可复用测试数据。

## 关键原则

- **测试即验证闭环**：写完让它自己跑、按结果迭代，并给证据。
- **沿用现有约定**：先看项目已有测试的框架/风格，别另起一套。
- **深度匹配**：小改动测核心行为即可；别为不可能发生的情况写测试（过度也是反模式）。

## 参考

- [references/test-principles.md](references/test-principles.md) — 金字塔 / AAA / FIRST / 行为 vs 实现 / mock 何时用
- [references/anti-patterns.md](references/anti-patterns.md) — 常见测试坏味道 + 修法
