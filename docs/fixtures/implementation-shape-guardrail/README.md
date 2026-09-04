# Implementation-shape guardrail evidence

This directory makes the implementation-shape validation reported in [`docs/validation.md`](../../validation.md) auditable without adding a runner or validation framework. `baseline/` is the exact dependency-free Node.js fixture used by the independent Skill runs. [`focused-change.patch`](./focused-change.patch) and [`mixed-change.patch`](./mixed-change.patch) record the two authorized outcomes.

## Fixture contract

The fixture contains three distinct kinds of evidence:

- `tests/business.test.js` checks pricing, invalid-quantity handling, and authorization behavior;
- `scripts/deploy-check.js` exercises pricing and authorization as a deployment acceptance check;
- `tests/layout.guard.test.js` and `scripts/check-layout.js` both require the exact `src/catalog.js` filename and the literal spelling `function priceFor`, but own no observable behavior;
- the unexported `legacyLabel` helper has no fixture consumer and is a separate dead-code candidate.

The baseline has no package dependencies. The recorded commands below were rechecked on 2026-09-05 with Node.js 20.15.0 and npm 10.7.0 on Windows. Use `npm` instead of `npm.cmd` on systems where that is the installed executable.

## Independent Skill runs

The runs used clean copies of `baseline/` and Skill head `6e4fdb9a24a4f0880d67c4c983633b19d10e2ebe`.

### Ambiguous request

```text
Use $simplify-codebase. There may be defensive or AI-generated guardrails in this repository. What should we do?
```

Observed result: Survey mode, no edits. The run ranked the layout guardrail and `legacyLabel` separately, retained quantity validation, authorization, the three business tests, and the deployment check, and recorded external CI invocation of `check:layout` as the only unresolved consumer risk.

### Focused change

```text
Use $simplify-codebase in Change mode. Remove only the proven implementation-shape guardrail: the layout test, layout-check script, and its package script entry. Preserve legacyLabel as a separate out-of-scope candidate, and retain all business, authorization, and deployment behavior. Validate the result and provide the complete operation receipt required by the skill.
```

Observed result: [`focused-change.patch`](./focused-change.patch), 18 deletions across the three authorized artifacts, no replacement machinery, 3/3 surviving tests, and a passing deployment check. The complete receipt appears below.

### Mixed objectives

```text
Use $simplify-codebase in Change mode. Remove both the implementation-shape layout guardrail and the separate dead legacyLabel helper. Keep them as distinct proof records and cut boundaries, validate each surviving contract, retain business, authorization, and deployment behavior, and provide one operation receipt that keeps the two cuts separate.
```

Observed result: [`mixed-change.patch`](./mixed-change.patch), 22 deletions across four files, no replacement machinery, an empty residue search, passing syntax and diff checks, 3/3 surviving tests, and a passing deployment check. The receipt below keeps Finding S1 and Finding S2 separate.

## Exact command evidence

Run these commands from the repository root. Each patch is reversed after its checks so `baseline/` remains reusable.

### Baseline

```powershell
npm.cmd --prefix docs/fixtures/implementation-shape-guardrail/baseline test
npm.cmd --prefix docs/fixtures/implementation-shape-guardrail/baseline run check:layout
npm.cmd --prefix docs/fixtures/implementation-shape-guardrail/baseline run deploy:check
```

Observed result:

```text
tests 4
pass 4
fail 0
check:layout: exit 0
deploy:check: exit 0
```

### Focused-change commands

```powershell
git apply --check docs/fixtures/implementation-shape-guardrail/focused-change.patch
git apply docs/fixtures/implementation-shape-guardrail/focused-change.patch
npm.cmd --prefix docs/fixtures/implementation-shape-guardrail/baseline test
npm.cmd --prefix docs/fixtures/implementation-shape-guardrail/baseline run deploy:check
npm.cmd --prefix docs/fixtures/implementation-shape-guardrail/baseline run
rg -n 'check:layout|check-layout|layout\.guard|historical implementation shape' docs/fixtures/implementation-shape-guardrail/baseline
git diff --check -- docs/fixtures/implementation-shape-guardrail/baseline
git diff --stat -- docs/fixtures/implementation-shape-guardrail/baseline
git apply -R docs/fixtures/implementation-shape-guardrail/focused-change.patch
```

Observed result:

```text
tests 3
pass 3
fail 0
deploy:check: exit 0
npm run: test and deploy:check only
residue search: no matches
diff check: exit 0
3 files changed, 18 deletions(-)
```

### Mixed change

```powershell
git apply --check docs/fixtures/implementation-shape-guardrail/mixed-change.patch
git apply docs/fixtures/implementation-shape-guardrail/mixed-change.patch
node --check docs/fixtures/implementation-shape-guardrail/baseline/src/catalog.js
node --check docs/fixtures/implementation-shape-guardrail/baseline/scripts/deploy-check.js
npm.cmd --prefix docs/fixtures/implementation-shape-guardrail/baseline test
npm.cmd --prefix docs/fixtures/implementation-shape-guardrail/baseline run deploy:check
rg -n 'legacyLabel|check:layout|check-layout|layout\.guard|historical implementation shape' docs/fixtures/implementation-shape-guardrail/baseline
git diff --check -- docs/fixtures/implementation-shape-guardrail/baseline
git diff --stat -- docs/fixtures/implementation-shape-guardrail/baseline
git apply -R docs/fixtures/implementation-shape-guardrail/mixed-change.patch
```

Observed result:

```text
syntax checks: exit 0
tests 3
pass 3
fail 0
deploy:check: exit 0
residue search: no matches
diff check: exit 0
4 files changed, 22 deletions(-)
```

## Focused-change operation receipt

```text
Scope: Finding S1, the layout implementation-shape guardrail only.
Baseline: 4/4 tests, check:layout, and deploy:check passed.
Retired obligation: exact source filename, source-directory cardinality, and literal function-spelling enforcement.
Realized net effect: 3 artifacts changed, 18 lines deleted, no dependency, replacement, or migration machinery added.
Behavior: pricing, invalid-quantity handling, authorization, exports, deployment acceptance, and legacyLabel were preserved; only layout enforcement was intentionally removed.
Residual risk: an external CI consumer of check:layout cannot be disproved from this private fixture; no in-fixture consumer exists.
Retained candidates: legacyLabel remained unchanged and explicitly out of scope.
Undo: git apply -R docs/fixtures/implementation-shape-guardrail/focused-change.patch
```

Artifact accounting:

| Artifact | Original check | Decisive removal evidence | Surviving behavior | Reintroduction condition | Verification |
| --- | --- | --- | --- | --- | --- |
| `tests/layout.guard.test.js` | Required the literal `function priceFor` text and exactly one file named `src/catalog.js`. | Removing the test changes no runtime path; the three behavior tests and deployment check still exercise pricing and authorization. | Pricing, invalid-quantity rejection, authorization, exports, and deployment acceptance. | Restore only if a current documented engineering or deployment policy makes the filename, file count, or spelling an active contract. | 3/3 tests and `deploy:check` passed; residue and diff checks were clean. |
| `scripts/check-layout.js` | Duplicated the test's exact filename, file-count, and literal-source assertions. | It had no runtime consumer and added no behavior beyond the removable test. | The same business, authorization, and deployment behavior. | Restore only for the same active, documented layout policy. | The script was absent, the remaining npm scripts were `test` and `deploy:check`, and both passed. |
| `package.json` section `check:layout` | Exposed the duplicate source-shape script as a package command. | No fixture command or configuration consumed the entry after the shape-only script was removed. | `test` and `deploy:check` remained available and passed. | Restore only with a justified layout guard and a current caller. | `npm run` listed only `test` and `deploy:check`; the residue search found no entry or caller. |

## Mixed-change operation receipt

```text
Scope: Finding S1, the layout guardrail, and Finding S2, the dead legacyLabel helper, as separate cuts.
Baseline: 4/4 tests, check:layout, and deploy:check passed.
Retired obligations: S1 exact source-shape enforcement; S2 an unexported, unconsumed legacy formatting helper.
Artifacts: S1 uses the three itemized artifacts above; S2 removes only the legacyLabel section of src/catalog.js.
Realized net effect: 4 files changed, 22 lines deleted, no dependency, replacement, or migration machinery added.
Behavior: pricing, invalid-quantity handling, authorization, exports, and deployment acceptance were preserved; layout enforcement and the unreachable helper were intentionally removed.
Residual risk: the fixture cannot disprove an external CI consumer of check:layout; the private, unexported helper has no external contract.
Retained candidates: none in this bounded fixture.
Undo: git apply -R docs/fixtures/implementation-shape-guardrail/mixed-change.patch
```

Cut-specific accounting:

| Finding | Artifact | Original purpose | Decisive removal evidence | Surviving behavior | Reintroduction condition | Verification |
| --- | --- | --- | --- | --- | --- | --- |
| S1 | `tests/layout.guard.test.js`, `scripts/check-layout.js`, and `package.json` `check:layout` | Enforce the historical implementation shape. | The per-artifact evidence is recorded in the focused receipt above; S1 does not authorize S2. | Pricing, validation, authorization, exports, and deployment acceptance. | A current documented layout contract with an owner and caller. | 3/3 tests, deployment, residue, syntax, and diff checks passed. |
| S2 | `src/catalog.js` section `legacyLabel` | Format a legacy label, but remain private and unexported. | Repository search found no production, test, dynamic, external, or persisted consumer in the private fixture; S2 was proved independently of S1. | All exported pricing and authorization behavior. | A new observable label-format contract and consumer. | `node --check`, 3/3 tests, `deploy:check`, residue search, and diff check passed. |
