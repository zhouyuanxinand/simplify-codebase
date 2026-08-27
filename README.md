# Simplify Codebase

[English](./README.en.md)

`simplify-codebase` 是一个帮助 Agent 调查和实施代码简化的 Skill。

这里说的简化，不是尽量删除更多代码，而是减少项目需要长期保持一致的状态、接口、兼容路径和抽象。默认先做只读调查；只有用户明确要求修改时才会动文件。证据不够就保留。

## 适合什么时候用

- 一个功能已经下线，想确认还有哪些代码、测试或文档残留；
- 多个字段或状态可能在表达同一件事；
- 某个接口、兼容分支或抽象看起来已经没有真实消费者；
- 项目积累了一段时间，想做一次只读的全库简化审计；
- 需要复核其他分支、PR 或 Agent 提出的清理建议。

它不适合普通代码 Review、格式整理、性能优化或新功能开发。

## 工作方式

- `Survey`：只读调查，给出候选、反证、盲区和下一步需要的证据。
- `Change`：在用户明确授权后实施修改，并给出验证结果和撤销路径。
- `Focused`：调查一个指定的子系统、状态机或疑似重复点。
- `Broad`：分区覆盖整个仓库，不因为先找到一个候选就提前结束。

每个候选都会检查生产、测试、动态和外部消费者，以及持久化、兼容性和历史决策。共享文件里的 selector、字段或注册项也会单独确认所有权。

## 安装

让 Codex 安装：

```text
Install the simplify-codebase skill from https://github.com/tt-a1i/simplify-codebase
```

或者手动克隆：

```bash
git clone https://github.com/tt-a1i/simplify-codebase.git \
  ~/.codex/skills/simplify-codebase
```

安装后新建一个任务，让 Skill 目录重新加载。其他支持 `SKILL.md` 的 Agent 环境可以把本仓库放进各自的 Skill 目录。

## 使用

只读审计整个仓库：

```text
使用 $simplify-codebase 审计这个仓库，列出最安全、收益最高的简化候选。不要修改文件。
```

调查一个具体问题：

```text
使用 $simplify-codebase 判断这些 readiness 标志是在表达不同的生命周期保证，还是重复状态。
```

实施一个已经证明安全的修改：

```text
使用 $simplify-codebase 删除一个高置信度的偶然复杂度来源。保留现有契约，完成验证，并给出撤销路径。
```

## 边界

删除仍然可达的能力、已支持接口、持久化表示或兼容路径，仍然属于产品决策。Skill 会指出影响，但不会替用户做这个决定。

开发时的行为验证记录在 [docs/validation.md](./docs/validation.md)。

## License

[MIT](./LICENSE)
