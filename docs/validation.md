# Validation / 验证记录

本文档记录 `simplify-codebase` 在首次开源前完成的行为验证。它不是永久质量声明；仓库和 Agent 运行时变化后，应重新验证相关结论。

This document records the behavioral validation completed before the initial open-source release. It is not a permanent quality claim; relevant results should be rechecked as repositories and Agent runtimes evolve.

## Scenarios / 场景

### Change

- Fixture: a small JavaScript stateful component with duplicated lifecycle state.
- Expected behavior: remove only the redundant representation while preserving dynamic and persisted behavior.
- Result: one source file changed; all 3 tests passed; no unrelated files changed.

### Broad

- Fixture: a clean 973-file production repository containing Python and TypeScript/TSX.
- Expected behavior: partition the repository, investigate beyond obvious unused symbols, and record retained and unresolved surfaces.
- Result: the audit covered the main runtime and repository domains, found frontend and backend candidates, retained live compatibility and lifecycle boundaries, and left the worktree clean.

### Integration and decision records

- Fixture: findings imported from another branch, including one valid cleanup, one unsafe compatibility removal, and one historical design record.
- Expected behavior: revalidate every finding instead of copying conclusions; preserve immutable decision history.
- Result: all 3 findings were mapped to a disposition; the compatibility decoder was retained; the historical ADR body stayed unchanged; both tests passed.

### Shared-artifact boundary

- Fixture: a retired frontend surface sharing a stylesheet with a surviving component.
- Expected behavior: prove the cut below file granularity.
- Result: the focused audit found 14 candidate-exclusive CSS classes, identified removable members inside mixed selectors, retained the shared stylesheet and surviving component, and found two stale documentation references.

### Implementation-shape guardrail

- Fixture: a small Node.js package with pricing and authorization behavior, an unexported dead helper, a deployment check, and a test plus script that asserted an exact source filename and function spelling.
- Ambiguous request: an independent Skill run was told only that defensive or AI-generated guardrails might exist. It selected Survey mode, made no edits, ranked the implementation-shape guard separately from the dead helper, and retained the quantity validation, authorization boundary, business tests, and deployment check.
- Focused change: an explicitly authorized run removed only `tests/layout.guard.test.js`, `scripts/check-layout.js`, and the `check:layout` package entry. The baseline passed 4 tests plus the layout and deployment checks; afterward all 3 surviving behavior tests and the deployment check passed. The operation receipt named every deleted artifact, its original check, the removal evidence, surviving behavior, reintroduction condition, verification, and undo path.
- Mixed objectives: an explicitly authorized run handled the shape guard and dead helper as separate proof records and cut boundaries. It removed 22 lines across 4 files with no replacement machinery, found no residue, passed syntax and diff checks, passed all 3 surviving behavior tests, and preserved the deployment check. The final receipt kept both cuts separate.
- Environment note: the Windows fixture runner used `npm.cmd` because the bare `npm` shim was unavailable; this did not change the fixture or expected behavior.

## Mechanical checks / 机械检查

- Skill structure and frontmatter validation;
- YAML metadata validation;
- internal Markdown link validation;
- Markdown lint with line-length enforcement disabled for prose and long commands;
- upstream-name and attribution scan;
- clean-worktree checks for read-only scenarios.

## Known limits / 已知边界

- These tests establish behavior for the exercised scenarios, not every language, build system, or Agent runtime.
- Dynamic consumers outside the inspected repository can remain unknowable without external evidence.
- Deployment, production health, and end-user acceptance remain separate gates from repository tests.
- A user must still authorize product or compatibility changes.
