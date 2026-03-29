---
name: rule-ab-testing
description: A/B tests one Cursor project rule by running the same tasks with the rule enabled vs disabled, then documents scenarios, both outcomes, and resolution. Use when comparing rule on vs off, before merging a risky rule, or when debugging agent behavior tied to a single .mdc rule.
---

# Skill: Rule A/B testing

## A/B definition (mandatory)

- **Variant A:** the rule under test is **enabled** (normal project state — file present and active in Cursor as usual).
- **Variant B:** the **same** rule is **disabled** (temporarily not applied: rename `.mdc` → `.mdc.off`, move the file out of `.cursor/rules/`, or use Cursor’s rule toggle — **record which mechanism** in the scenario write-up).

All scenarios use **the same user task**; only the difference is rule on vs rule off.

## When to use

- Before merging a new or heavily edited rule.
- When a rule may over-constrain the model or cause noisy refusals.
- When debugging unexpected agent behavior tied to one `.cursor/rules/*.mdc` file.

## Inputs

- Path to the rule file under test (e.g. `.cursor/rules/conventions.mdc`).
- **1–3 fixed** task prompts — identical strings for both A and B runs.
- Optional: files or globs the tasks should focus on.
- Chosen **reversible** way to disable the rule for variant B (document before running B).

## Steps

1. **Branch** — work on a dedicated git branch; keep the working tree easy to reset between A and B.
2. **Variant A (rule enabled)** — ensure the rule file is active. For each scenario prompt, run the task; record observable behavior (files touched, compliance with intent, errors, refusals).
3. **Baseline** — restore the same starting state (revert changes from A, or stash, or reset tracked files) so B compares fairly. Re-enable the rule if you toggled it only after A.
4. **Variant B (rule disabled)** — disable the rule using the chosen mechanism. Re-run **identical** scenario prompts; record the same observations.
5. **Re-enable** — turn the rule back on unless the **Resolution** explicitly keeps it off or replaces it.
6. **Verification** — if scenarios change code, run the same checks for both variants where applicable (see `.cursor/rules/testing.mdc`: e.g. `yarn test:typecheck`, `yarn test:code`, targeted `yarn test:app`).

## Documentation template (required per scenario)

Copy and fill one block per scenario in the log (see **Where to log**).

```markdown
### Scenario <id>

- **Prompt / task** — (exact text)
- **Setup** — Rule: `<path>`; A = enabled; B = disabled via: `<mechanism>`

**Results — A (rule on)**

- …

**Results — B (rule off)**

- …

**Resolution**

- Decision: (keep enabled / keep disabled / edit rule — describe)
- Rationale: …
- Follow-ups: …
```

## Where to log

- **Default:** `docs/experiments/rule-ab-YYYY-MM-DD.md` (create `docs/experiments/` if missing), **or** append a dated subsection to `docs/memory/decisionLog.md`.
- **Override:** use a path the user gives instead.

## Outputs

- Path to the written log file.
- Short executive summary: whether the rule stays on, off, or is revised.

## Safety

- Do **not** disable `security.mdc` or weaken `do-not-touch.mdc` for an experiment without **explicit** user approval.
- Prefer reversible disables (rename to `.mdc.off`, temporary move) and commit only the **log + final rule state**, not half-finished toggles.
- Re-enable the rule after B unless **Resolution** says otherwise.
