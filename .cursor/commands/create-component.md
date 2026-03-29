# create-component

**Scaffold** a new UI piece. Do **not** refactor unrelated code, add Storybook, or change core `types.ts` unless the user asks.

## Where to put it

- **Default:** `packages/excalidraw/components/` for editor UI.
- **`excalidraw-app/`** only if the user explicitly wants app-shell UI.
- If the user gives no path and context is unclear, **ask once** for the target folder; otherwise infer from open files / conversation.

## Examples

1. **Input:** “Add a `ToolHint` component for the editor toolbar.”  
   **Artifacts:** `packages/excalidraw/components/ToolHint.tsx` with `export type ToolHintProps = { … }` / `export const ToolHint`, optional `ToolHint.test.tsx` with a meaningful assertion (e.g. renders label text). Default folder: `packages/excalidraw/components/`.

2. **Input:** “Scaffold `ColorSwatch` in the app shell.”  
   **Artifacts:** `excalidraw-app/components/ColorSwatch.tsx` + `ColorSwatchProps`, tests only if requested or clearly needed; same naming rules.

## Rules while generating

| Rule | Apply |
|------|--------|
| `conventions.mdc` | **New/changed** code: prefer functional components + hooks and **named exports** (avoid new `export default`). **Legacy** class/default-export patterns exist in the repo — do not rewrite unrelated files; follow the same preferences for `excalidraw-app/**` even when the rule glob is editor-only. Props: `{ComponentName}Props`; PascalCase `MyWidget.tsx`; kebab-case utilities; colocated tests when they add signal; strict TS — no unnecessary `any` / `@ts-ignore`; `import type` where appropriate. |
| `architecture.mdc` | In `packages/excalidraw/**`: use existing state patterns (`actionManager`, etc.) — **not** Redux/Zustand/MobX; do not move canvas drawing to React DOM. |
| `do-not-touch.mdc` | **Do not** edit listed paths unless the user explicitly requires it and accepts risk. |

## Deliverables

1. **Component file(s)** — minimal implementation: props, structure, sensible defaults; if interactive, basic a11y (labels, keyboard where obvious).
2. **Colocated test** — when you add `ComponentName.test.tsx`, include at least one **meaningful** check (render + behavior, props contract, callback, or a11y); otherwise skip the test file rather than placeholder-only assertions.
3. **Imports/exports** — only what is needed; **wire into a parent** only if the user asked for integration.

## Out of scope (unless requested)

- i18n / locale keys, Storybook, broad refactors, edits to `packages/excalidraw/types.ts` core types.

After adding files, run checks per `testing.mdc`: `yarn test:typecheck`, `yarn test:code`, and targeted `yarn test:app` when tests were added.
