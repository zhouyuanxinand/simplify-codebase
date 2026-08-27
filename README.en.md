# Simplify Codebase

[中文](./README.md)

`simplify-codebase` is an Agent Skill for investigating and applying codebase simplifications.

Simplification here means reducing the states, interfaces, compatibility paths, and abstractions a project must keep coherent over time. It does not optimize for deleted lines. Read-only investigation is the default; files change only when the user explicitly asks for it. Insufficient evidence means the candidate stays.

## When to use it

- A retired feature may still have code, tests, or documentation behind it.
- Multiple fields or states may represent the same fact.
- An interface, compatibility branch, or abstraction may no longer have a real consumer.
- A repository has accumulated enough history to justify a read-only simplification survey.
- Cleanup findings from another branch, PR, or Agent need independent verification.

It is not intended for general code review, formatting, performance tuning, or feature development.

## Modes

- `Survey`: inspect without editing; return candidates, counter-evidence, blind spots, and the next facts needed.
- `Change`: apply an explicitly authorized cut, then report validation and an undo path.
- `Focused`: investigate one named subsystem, state machine, or suspected duplication.
- `Broad`: partition and cover the repository instead of stopping at the first plausible candidate.

Each candidate is checked against production, test, dynamic, and external consumers, along with persistence, compatibility, and design history. Candidate-owned selectors, fields, or registry entries inside shared files are accounted for separately.

## Install

Ask Codex to install it:

```text
Install the simplify-codebase skill from https://github.com/tt-a1i/simplify-codebase
```

Or clone it manually:

```bash
git clone https://github.com/tt-a1i/simplify-codebase.git \
  ~/.codex/skills/simplify-codebase
```

Start a new task after installation so the Skill catalog refreshes. Other Agent environments that support `SKILL.md` can place the repository in their own Skill directory.

## Use

Survey a repository without editing it:

```text
Use $simplify-codebase to audit this repository and rank the safest high-impact simplification candidates. Do not modify files.
```

Investigate a specific concern:

```text
Use $simplify-codebase to determine whether these readiness flags represent distinct lifecycle guarantees or duplicated state.
```

Apply a proved simplification:

```text
Use $simplify-codebase to remove one high-confidence source of accidental complexity. Preserve the current contract, validate the change, and provide an undo path.
```

## Boundaries

Removing a reachable capability, supported interface, persisted representation, or compatibility path remains a product decision. The Skill reports the consequence but does not make that decision for the user.

Behavioral validation from development is recorded in [docs/validation.md](./docs/validation.md).

## License

[MIT](./LICENSE)
