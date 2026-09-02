<div align="center">

# Simplify Codebase

**先证明，再删除。让代码库少维护一些事实、状态与契约。**

[![Agent Skill](https://img.shields.io/badge/Agent-Skill-22c55e?style=flat-square)](./SKILL.md)
[![License: MIT](https://img.shields.io/badge/License-MIT-0f172a?style=flat-square)](./LICENSE)
[![中文](https://img.shields.io/badge/README-中文-06b6d4?style=flat-square)](./README.md)
[![English](https://img.shields.io/badge/README-English-64748b?style=flat-square)](./README.en.md)

<img src="./assets/hero.png" alt="复杂的软件结构经过证据验证后，收敛为更小、更清晰的系统" width="100%" />

</div>

`simplify-codebase` 是一个面向现有代码库的 Agent Skill。它有两个互不混用的处理目标：识别并安全移除偶然复杂度，或清理 AI 修改后留下的维护性防回退层；两者都保护仍然有效的行为、边界与兼容性。

它不追求“删得多”。它关心的是：一次改动能否减少团队今后必须持续保持一致的概念和义务。

## AI 防回退层清理

AI 修改可能留下只保护某次实现形态的测试、构建/CI 守卫、静态扫描或清单。选择此目标后，Skill 会先要求选择类别，再追踪真实消费者与边界；类别只授权调查，不授权删除仍在保护业务、API、安全、数据完整性或真实部署行为的代码。

当用户同时要求普通简化和防回退层清理时，Skill 会分别维护候选、删除边界与验证结果，避免把两种风险混在同一批修改中。

## 为什么需要它

代码库里的冗余很少只是“某个函数没人调用”。它也可能是重复状态、失去所有者的抽象、只剩测试消费的接口、早已无效的兼容路径，或者被保留在共享文件中的半截功能。

静态检查可以提供线索，但不能单独证明一项删除是安全的。这个 Skill 会继续追踪运行时消费者、动态注册、持久化格式、公共接口、历史决策与验证边界，再决定应该删除、合并、保留，还是标记为暂时无法判断。

> **核心原则：** 删除代码行只是结果。真正的收益是删除一个需要长期维护的事实、状态、契约或概念。

## 工作方式

| | 聚焦范围 `Focused` | 全库范围 `Broad` |
| --- | --- | --- |
| **只读审计 `Survey`** | 深挖一个子系统、状态机或疑似重复点 | 分区覆盖整个仓库，给出候选、反证与盲区 |
| **授权修改 `Change`** | 证明并完成一个明确的简化边界 | 按所有权边界分批修改，每批独立验证 |

每个候选都要形成一份证明记录：

- 它增加了什么维护负担；
- 生产、测试、动态和外部消费者分别是谁；
- 完整删除边界在哪里，包括共享文件内部的成员；
- 会放弃什么可观察行为或兼容性；
- 哪个最小验证可以揭示错误删除；
- 减少的复杂度是否大于新引入的迁移或替代机制。

## 它会谨慎对待

- 公共 API、动态加载和插件注册；
- 数据格式、迁移、回放与向后兼容；
- 授权、隔离、输入校验和数据丢失防护；
- 并发、取消、清理和生命周期所有权；
- 生成文件、共享资源和仓库外消费者；
- 仍然有效的 ADR、RFC 与架构约束。

发现真实消费者、边界尚未查清，或者简化只是把复杂度搬到别处时，它会建议保留，而不是为了输出结果强行删除。

## 安装

让 Codex 安装：

```text
Install the simplify-codebase skill from https://github.com/tt-a1i/simplify-codebase
```

也可以手动安装到 Codex 的用户级 Skill 目录：

```bash
git clone https://github.com/tt-a1i/simplify-codebase.git \
  ~/.codex/skills/simplify-codebase
```

安装后请新建一个任务，让 Skill 目录重新加载。其他支持 `SKILL.md` 的 Agent 环境可将本仓库放入各自的 Skill 目录。Codex、Claude Code、Cursor、GitHub Copilot、Cline、Gemini CLI 与 OpenCode 的目录、优先级和验证方式见 [跨 Harness 兼容性](./docs/harness-compatibility.md)。

## 使用

### 审计整个仓库，不改文件

```text
使用 $simplify-codebase 审计这个仓库，列出最安全、收益最高的简化候选。不要修改文件。
```

### 调查一个具体问题

```text
使用 $simplify-codebase 判断这些 readiness 标志是在表达不同的生命周期保证，还是重复状态。
```

### 实施一个已经证明安全的简化

```text
使用 $simplify-codebase 删除一个高置信度的偶然复杂度来源。保留仍然有效的契约，完成验证，并给出操作回执和撤销路径。
```

### 合并其他分支或 Agent 的发现

```text
使用 $simplify-codebase 复核并整合这个 PR 中的简化建议。保留证据，不保留候选数量。
```

### 清理 AI 修改防回退层

```text
使用 $simplify-codebase 处理这个仓库。先让我选择：普通代码简化，还是 AI 修改防回退层清理；不要修改文件。
```

```text
选择 AI 防回退层清理。清理测试防回退层、构建部署与 CI 防回退层、静态检查脚本与清单。保留业务、API、安全、数据完整性和真实部署行为。
```

运行时重试、回退、修复和恢复路径风险较高，只有在用户明确授权且证据证明不再保护真实边界时才处理。

## 输出是什么样的

只读审计会交付覆盖范围、排序后的证明记录、重要反例、未决问题和下一条所需证据。

修改任务会额外交付实际变更、分层验证结果、剩余风险、操作回执与可执行的撤销路径。一次小范围测试通过，不会被包装成完整的运行时或用户验收。

如果用户授权生成 Handoff，防回退清理必须逐项记录每个删除文件、删除符号或删除区段：原本作用、防回退职责、消费者证据、为何安全删除、保留的行为、重新引入条件和验证结果。

## 仓库结构

```text
.
├── SKILL.md                    # 主工作流与判断标准
├── agents/openai.yaml          # Agent 展示与调用元数据
├── references/
│   ├── investigation.md        # 全库调查与候选发现
│   ├── boundaries-and-lifecycle.md
│   ├── execution-and-recovery.md
│   ├── decision-records.md
│   ├── integrating-findings.md
│   ├── defensive-categories.md
│   └── defensive-proof-and-delivery.md
├── docs/validation.md          # 行为验证与质量证据
├── docs/harness-compatibility.md # 跨平台安装与验证
├── scripts/verify_harness_contract.py # 可移植契约校验
└── assets/hero.png             # 原创 Hero 视觉
```

## 质量与边界

这个版本经过 Change、Broad、Integration 和 Decision-record 场景验证，也在一个 973 文件的 Python + TypeScript 项目上完成过全库审计。测试方法与已知边界记录在 [docs/validation.md](./docs/validation.md)。跨 harness 的目录与元数据契约见 [docs/harness-compatibility.md](./docs/harness-compatibility.md)。

Skill 不能替代产品决策。删除仍然可达的能力、已支持接口、持久化表示或兼容路径时，仍需由使用者明确授权。AI 防回退模式也不能把安全校验、凭据处理、数据完整性、访问隔离或持久化恢复误判为普通防回退代码。

## 贡献

欢迎提交 Issue 和 PR。请优先提供失败案例、遗漏的消费者、错误删除风险或可以复现的验证缺口；这比单纯增加更多规则更有价值。

## License

[MIT](./LICENSE)
