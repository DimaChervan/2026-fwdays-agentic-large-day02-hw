# review-code

Perform a **code review** only. Do **not** rewrite unrelated code or apply fixes unless the user explicitly asks.

## Scope

- Default: **staged + unstaged** changes (`git diff` / `git diff --cached`). If the user names files, a branch, or a commit range, use that scope instead.
- Read `.cursor/rules/` and apply them to the diff.

## Rules to align with

| Rule | When |
|------|------|
| `architecture.mdc` | `packages/excalidraw/**` |
| `conventions.mdc` | `packages/**/*.ts`, `packages/**/*.tsx` |
| `security.mdc` | `excalidraw-app/**`, `packages/excalidraw/data/**` |
| `do-not-touch.mdc` | always — flag edits to listed paths without strong justification |
| `testing.mdc` | always — tests / `yarn test:*` expectations |
| `memory-bank.mdc` | if the change affects something documented under `docs/memory/` |

## Checklist

1. **Correctness** — logic, regressions, edge cases.
2. **Types** — avoid unnecessary `any` / `@ts-ignore` (per `conventions.mdc`).
3. **Tests** — missing coverage, needed snapshot updates; cite `testing.mdc` / `yarn test:typecheck`, `yarn test:code`, `yarn test:app`, `yarn test:update`, `yarn test:all` as relevant.
4. **Security** (if diff touches app or `packages/excalidraw/data/**`) — secrets, `postMessage` / origin, untrusted JSON, XSS, crypto — per `security.mdc`.
5. **Protected files** — warn if `do-not-touch.mdc` paths change without clear need.

## Output

1. **Summary** — a few sentences.
2. **Findings** — grouped by severity:
   - **Must-fix**
   - **Should-fix**
   - **Nit**  
   Each item: file path + concrete, actionable note.
3. **Verdict** — one of: `approve` / `approve with nits` / `request changes`.

Tone: professional and specific; no filler praise. Optional: one line — further commands in [`AGENTS.md`](../../AGENTS.md).
