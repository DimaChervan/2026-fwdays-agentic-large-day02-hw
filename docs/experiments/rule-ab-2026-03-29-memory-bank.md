# Rule A/B test: `memory-bank.mdc`

**Rule:** `.cursor/rules/memory-bank.mdc`  
**Mechanism for B:** rename `memory-bank.mdc` → `memory-bank.mdc.off` (reversible).

## Fixed prompt (identical for A and B)

> Vitest in `docs/memory/techContext.md` does not match root `package.json` `devDependencies.vitest`. Fix the Vitest version in the Core stack table and align memory-bank docs as appropriate.

## Controlled drift

- Before each variant: `techContext.md` had **Vitest `3.0.5`** (wrong); root `package.json` has **`vitest`: `3.0.6`**.
- `progress.md` **As of** was **2026-03-28** at start of both variants.

---

### Scenario 1

- **Prompt / task** — (see above)
- **Setup** — Rule: `.cursor/rules/memory-bank.mdc`; **A** = enabled; **B** = disabled via rename to `memory-bank.mdc.off`

**Results — A (rule on)**

- Updated `docs/memory/techContext.md`: Vitest `3.0.5` → **`3.0.6`** (matches `package.json`).
- Updated `docs/memory/progress.md`: **As of** **2026-03-28** → **2026-03-29** (re-verification / snapshot hygiene per memory bank and `progress.md` “How to update”).

**Results — B (rule off)**

- Updated `docs/memory/techContext.md`: Vitest `3.0.5` → **`3.0.6`** only.
- **Did not** bump `docs/memory/progress.md` **As of** (left **2026-03-28**) — narrow interpretation of “fix the table” without cross-file snapshot discipline.

**Resolution**

- **Decision:** **Keep rule enabled** (`memory-bank.mdc` active).
- **Rationale:** With the rule off, toolchain drift can be “fixed” in one file while the progress snapshot stays stale; the rule nudges updating related `docs/memory/` files and verification dates when reconciling versions.
- **Follow-ups:** Optional: add this experiment path to `AGENTS.md` or `decisionLog.md` if you want the procedure discoverable; not done in this run.

---

## Final repo state (hygiene)

- `memory-bank.mdc` **re-enabled**.
- `techContext.md` shows Vitest **3.0.6**.
- `progress.md` **As of** set to **2026-03-29** after the experiment so the tree is consistent.
