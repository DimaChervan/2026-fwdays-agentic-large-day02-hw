# create-component

**Scaffold** a new UI piece. Do **not** refactor unrelated code, add Storybook, or change core `types.ts` unless the user asks.

## Where to put it

- **Default:** `packages/excalidraw/components/` for editor UI.
- **`excalidraw-app/`** only if the user explicitly wants app-shell UI.
- If the user gives no path and context is unclear, **ask once** for the target folder; otherwise infer from open files / conversation.

## Rules while generating

| Rule | Apply |
|------|--------|
| `conventions.mdc` | Functional components + hooks only; props type `{ComponentName}Props`; **named exports** only; PascalCase file `MyWidget.tsx`; kebab-case for non-component utilities; colocated `MyWidget.test.tsx`; strict TS — no unnecessary `any` / `@ts-ignore`; `import type` where appropriate. |
| `architecture.mdc` | In `packages/excalidraw/**`: use existing state patterns (`actionManager`, etc.) — **not** Redux/Zustand/MobX; do not move canvas drawing to React DOM. |
| `do-not-touch.mdc` | **Do not** edit listed paths unless the user explicitly requires it and accepts risk. |

## Deliverables

1. **Component file(s)** — minimal implementation: props, structure, sensible defaults; if interactive, basic a11y (labels, keyboard where obvious).
2. **Colocated test** — optional stub with smoke render or trivial assertion when conventions call for `ComponentName.test.tsx`.
3. **Imports/exports** — only what is needed; **wire into a parent** only if the user asked for integration.

## Out of scope (unless requested)

- i18n / locale keys, Storybook, broad refactors, edits to `packages/excalidraw/types.ts` core types.

After adding files, run checks per `testing.mdc`: `yarn test:typecheck`, `yarn test:code`, and targeted `yarn test:app` when tests were added.
