---
name: no-bs-review
display_name: 不废话代码审查
display_name_en: No-BS Code Review
version: 1.0.0
description: 按务实而严格的工程规范进行代码质量审查与合并前把关：红线无条件必改、黄线讲清理由、坚决反过度设计、项目既有惯例优先。当用户提到"代码审查"、"review"、"CR"、"帮我看看这段代码"、"检查下代码"、"合并前把关"、"代码质量评估"，或贴出、指向任何业务代码并要求评估质量时使用——即使用户没有说出"审查"两个字。输出按严重程度分级的问题清单。只评估不改动代码，除非用户明确要求修复。不用于开发新功能，不用于执行重构。
description_zh: 按务实而严格的工程规范进行代码质量审查与合并前把关：红线无条件必改、黄线讲清理由、坚决反过度设计、项目既有惯例优先。当用户提到"代码审查"、"review"、"CR"、"帮我看看这段代码"、"检查下代码"、"合并前把关"，或贴出代码要求评估质量时使用。只输出评估意见不修改代码，不用于开发新功能或执行重构。
description_en: Pragmatic and strict engineering code review for general codebases. Enforces redlines unconditionally, explains rationale for yellow lines, opposes over-engineering, and prioritizes project conventions. Outputs a tiered issue list. Does not modify code or implement new features.
category: coding
author: ZHANGCHAO
examples_zh:
  - "做下合并前代码审查，按红线与黄线列出必须修复项"
  - "帮我CR这段核心业务代码，检查有没有过度设计和并发风险"
  - "审查这个模块的实现，重点关注代码复杂度、性能陷阱与内存泄漏"
examples_en:
  - "Perform a pre-merge code review with tiered redline issues"
  - "Review this core business logic and check for over-engineering or concurrency risks"
  - "Review this module implementation for code complexity, performance traps, and memory leaks"
tags:
  - code-review
  - cr
  - pre-merge
  - clean-code
  - anti-overengineering
  - code-quality
---

# 不废话代码审查（No-BS Review）

> 版本：1.0 · 2026/10/24

## 这个技能做什么

对提交或待合并的业务代码做合并前质量把关。**只输出评审意见，不修改代码**，除非用户明确说"顺手改掉"。

所有裁决基于同一条优先级链，问题之间冲突时靠它取舍：

```
正确 > 可读 > 可调试 > 项目一致 > 可维护 > 简洁 > 性能 > 优雅
```

安全属于"正确"的一部分；达不到业务性能指标同样属于"不正确"。"高级抽象"不在列表中——它是手段，不是目标。

## 审查前先确认三件事

直接跳到逐行挑错是最常见的错误。以下顺序不能省：

1. **技术栈与项目惯例**：看同目录文件、同类功能的既有写法、已有公共组件。项目现有风格优先于个人审美——按项目惯例写的代码，即使不合本规范也不报，除非用户点名要求全面整改。
2. **区分新建与存量**：有 git diff 先看 diff。黄线指标只对新建文件生效；本次改动的存量文件跳过黄线，只查红线、正确性与安全。
3. **任务性质**：存量项目的普通需求，严格审第三方依赖新增；新项目、新模块或明确的技术选型，允许业务所必需的合理依赖。

## 第一遍：红线（🔴 无条件必改，没有例外）

| 检查项 | 判据 |
| --- | --- |
| 嵌套层级 | if / for / try 嵌套超过 3 层 |
| Lambda 嵌套 | Lambda 内再嵌 Lambda，一律禁止 |
| 嵌套三元 | 三元表达式套三元，一律禁止 |
| 第三方依赖 | 存量项目普通需求中无理由新增；或有理由但未说明安全风险、许可证、版本兼容影响 |
| 正确性缺陷 | 边界没覆盖、异常路径丢失、并发隐患、明显逻辑错误 |
| 安全漏洞 | 命中 [references/security-performance.md](./references/security-performance.md) 清单任一项 |

## 第二遍：黄线（🟡 触发思考，不是"必须拆"）

| 检查项 | 参考值 |
| --- | --- |
| 单个方法 | ~60 行 |
| 单个文件（仅新建） | ~400 行 |
| 链式调用 | ~5 环，且每环表达式不超过一行 |
| Lambda 函数体 | ~3 行 |
| 泛型嵌套 | 2 层（如 `Map<String, List<Foo>>`） |

黄线触发后的唯一动作：让作者回到"3 句话自检"——能否用 3 句中文说清这段代码的执行流程？说得清就保持原样；说不清才建议拆分。

**为凑数字而拆分，本身就是违规**：把 70 行连贯方法机械切成三个 25 行、靠成员变量传状态，比原来更难读。100 行的扁平 switch 好过 30 行的三层嵌套循环。评审时按可读性判断，行数只是不准的代理指标。

天然豁免（不计黄线，也不要建议拆分）：switch / 表驱动映射 / 常量类 / 配置类；DTO、VO、Entity 等纯数据结构；测试中的连续断言；MyBatis XML 与 Mapper 接口；项目现有链式 API 惯例（如 MyBatis-Plus `LambdaQueryWrapper`）；存量文件。

## 第三遍：过度设计（🟡 或 💡）

以下模式出现时先质疑，再决定是否上报：

- 一个 if/else 能解决，却用了策略模式
- 一个 switch 能解决，却搭了多态体系
- 能用 `Map<String, Handler>` 却设计了 Registry
- 参数 ≤ 4 个却造了 Builder
- 能直接调用却包了一层；能用具体类型却用了泛型
- Spring 项目里出现 Domain Layer / CQRS / Event Sourcing 等额外分层（默认就三层：Controller → Service → Mapper/Repository）

增加抽象只有五个正当理由：同一变化点已有 ≥ 2 个实现；同一段逻辑重复 ≥ 3 次；分支超过 3 路且还在增长；测试确实需要隔离外部依赖；项目现有架构本来就要求这种结构。**不能凭"未来可能出现"提前设计。**

## 第四遍：语言与专项细则

本技能以全语言通用的工程规范为底座。针对特定技术栈，按本次代码的技术类型按需参考同级目录下的专项文件：

- Java → 深入参考 [references/java-review.md](./references/java-review.md)（Stream 单环复杂度 / Optional 规范 / 异常与日志保留现场）
- TypeScript / Vue → 深入参考 [references/frontend-review.md](./references/frontend-review.md)（类型复杂度判据 / Vue 3 组件规范 / 前端大列表与内存泄漏）
- 后端服务与数据层 → 必读 [references/security-performance.md](./references/security-performance.md)（注入、越权、并发及 N+1 查询等安全与性能陷阱）
- Python / Go / Rust 等其他语言 → 严格执行前三遍原则及安全清单；语言特有语法遵从该语言生态主流规范

## 输出格式（固定模板）

```markdown
## 审查结论：可合并 ｜ 修改后可合并 ｜ 需要重做
一句话说明整体判断。

### 🔴 必须修复（N 项）
1. `文件路径:行号` — 问题一句话。
   - 依据：违反的规则 + 一句话解释这条规则为什么存在
   - 建议：给出 Before/After 最小示例

### 🟡 建议关注（N 项）
（格式同上；注明：作者能说明理由的可以保留现状）

### 💡 顺带提示（最多 2 条）
只提紧邻修改点的明显小问题（魔法数、命名不达意等），不展开。

### 未检查项
如实列出本次没有信息或能力验证的点（如未运行测试、未核对数据库索引）。
```

输出纪律：

- 每条问题必须带"依据"，并解释规则背后的原因——让作者理解，而不是屈服。
- 没有问题的类别直接省略；全部干净就写"未发现问题"并给出结论，不要为了显得认真硬凑条目。
- 💡 顺带提示硬性上限 2 条，超出部分不报。
- 报告一律简体中文。
