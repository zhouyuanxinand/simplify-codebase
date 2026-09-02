<div align="center">

# Simplify Codebase

**Prove first. Delete second. Leave fewer facts, states, and contracts to maintain.**

[![Agent Skill](https://img.shields.io/badge/Agent-Skill-22c55e?style=flat-square)](./SKILL.md)
[![License: MIT](https://img.shields.io/badge/License-MIT-0f172a?style=flat-square)](./LICENSE)
[![中文](https://img.shields.io/badge/README-中文-06b6d4?style=flat-square)](./README.md)
[![English](https://img.shields.io/badge/README-English-64748b?style=flat-square)](./README.en.md)

<img src="./assets/hero.png" alt="A complex software system passing through an evidence gate and emerging smaller and clearer" width="100%" />

</div>

`simplify-codebase` is an Agent Skill with two separate objectives: safely remove accidental complexity from an existing codebase, or clean maintenance-only defensive layers left by AI-generated changes. Both objectives protect behavior, boundaries, and compatibility that still matter.

It does not optimize for deletion volume. It asks whether a change reduces the number of concepts and obligations a team must keep coherent over time.

## AI defensive-layer cleanup

AI-generated changes can leave tests, build/CI guards, static scans, and inventories that protect only one implementation shape. After this objective is selected, the Skill asks for a cleanup category and traces real consumers and boundaries. A category authorizes investigation, not deletion of a live business, API, security, data-integrity, or deployment contract.

When both objectives are requested, the Skill keeps their candidates, cut boundaries, and validation results separate instead of mixing the risks in one change batch.

## Why it exists

Codebase entropy is rarely just an unused function. It can be duplicated state, an ownerless abstraction, an interface consumed only by tests, an obsolete compatibility path, or half of a retired feature still embedded in a shared artifact.

Static analysis can surface leads, but it cannot prove a deletion safe by itself. This Skill follows runtime consumers, dynamic registration, persisted formats, public interfaces, design history, and verification boundaries before classifying a candidate as remove, merge, retain, or unresolved.

> **Core principle:** deleted lines are an outcome. The durable gain is deleting a fact, state, contract, or concept that no longer needs maintenance.

## How it works

| | `Focused` scope | `Broad` scope |
| --- | --- | --- |
| **`Survey` · read only** | Investigate one subsystem, state machine, or suspected duplication | Partition the repository and report candidates, counter-evidence, and blind spots |
| **`Change` · authorized edits** | Prove and complete one explicit simplification boundary | Work in independently validated ownership batches |

Every serious candidate receives a proof record covering:

- the maintenance burden it creates;
- production, test, dynamic, and external consumers;
- the complete cut, including candidate-owned members inside shared files;
- observable behavior or compatibility that would be surrendered;
- the smallest check capable of exposing an incorrect cut;
- whether complexity removed exceeds migration or replacement machinery added.

## Guardrails

The Skill treats these surfaces as first-class evidence:

- public APIs, dynamic loading, and plugin registration;
- stored formats, migrations, replay, and backward compatibility;
- authorization, isolation, validation, and data-loss protection;
- concurrency, cancellation, cleanup, and lifecycle ownership;
- generated artifacts, shared resources, and external consumers;
- current ADRs, RFCs, and architectural constraints.

When a real consumer exists, a boundary remains unresolved, or a proposal merely moves complexity elsewhere, the right result is to retain the code—not force a deletion.

## Install

Ask Codex to install it:

```text
Install the simplify-codebase skill from https://github.com/tt-a1i/simplify-codebase
```

Or clone it into the Codex user Skill directory:

```bash
git clone https://github.com/tt-a1i/simplify-codebase.git \
  ~/.codex/skills/simplify-codebase
```

Start a new task after installation so the Skill catalog refreshes. For other Agent environments that support `SKILL.md`, place the repository in that environment's Skill directory. See [Harness compatibility](./docs/harness-compatibility.md) for Codex, Claude Code, Cursor, GitHub Copilot, Cline, Gemini CLI, and OpenCode locations and verification.

## Use

### Audit a repository without editing it

```text
Use $simplify-codebase to audit this repository and rank the safest high-impact simplification candidates. Do not modify files.
```

### Investigate a specific concern

```text
Use $simplify-codebase to determine whether these readiness flags represent distinct lifecycle guarantees or duplicated state.
```

### Apply a proved simplification

```text
Use $simplify-codebase to remove one high-confidence source of accidental complexity. Preserve the surviving contract, validate it, and provide an operation receipt with an undo path.
```

### Integrate findings from elsewhere

```text
Use $simplify-codebase to verify and integrate the simplification findings from this PR. Preserve evidence, not finding counts.
```

### Clean AI defensive layers

```text
Use $simplify-codebase for this repository. First ask me to choose ordinary simplification or AI defensive-layer cleanup; do not edit files yet.
```

```text
Choose AI defensive-layer cleanup. Remove test guardrails, build/deployment/CI guardrails, and static-check scripts and inventories. Preserve business behavior, APIs, security, data integrity, and real deployment behavior.
```

Runtime retries, fallbacks, repairs, and recovery paths are high risk. Handle them only with explicit authorization and evidence that they protect no real boundary.

## What it returns

A read-only survey returns coverage, ranked proof records, important counterexamples, unresolved questions, and the next fact needed for each uncertainty.

A change task also returns the implemented cut, validation results by layer, remaining risk, an operation receipt, and an executable undo path. A narrow green check is never presented as complete runtime or user acceptance.

When a Handoff is authorized for defensive cleanup, it must list every deleted file, symbol, or section with its original role, defensive obligation, consumer evidence, removal rationale, retained behavior, reintroduction trigger, and verification result.

## Repository layout

```text
.
├── SKILL.md                    # Core workflow and decision rules
├── agents/openai.yaml          # Agent-facing metadata
├── references/
│   ├── investigation.md        # Broad investigation and discovery
│   ├── boundaries-and-lifecycle.md
│   ├── execution-and-recovery.md
│   ├── decision-records.md
│   ├── integrating-findings.md
│   ├── defensive-categories.md
│   └── defensive-proof-and-delivery.md
├── docs/validation.md          # Behavioral validation evidence
├── docs/harness-compatibility.md # Cross-harness installation and validation
├── scripts/verify_harness_contract.py # Portable contract check
└── assets/hero.png             # Original hero artwork
```

## Quality and boundaries

This version has been exercised in Change, Broad, Integration, and Decision-record scenarios, including a full survey of a 973-file Python + TypeScript project. See [docs/validation.md](./docs/validation.md) for the method and known limits, and [docs/harness-compatibility.md](./docs/harness-compatibility.md) for the cross-harness directory and metadata contract.

The Skill does not replace product judgment. Removing a reachable capability, supported interface, persisted representation, or compatibility path still requires explicit user authority. The AI defensive-layer mode must not misclassify security checks, credential handling, data integrity, access isolation, or durable recovery as routine guardrails.

## Contributing

Issues and pull requests are welcome. Reproducible failure cases, missed consumers, unsafe-deletion risks, and verification gaps are more valuable than adding rules without observed evidence.

## License

[MIT](./LICENSE)
