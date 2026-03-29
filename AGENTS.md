# AGENTS.md

## Project overview

This repository is the **Excalidraw monorepo**: an open-source diagram editor shipped as the **`@excalidraw/excalidraw`** React library and the **`excalidraw-app`** web application (Vite). **Package manager: Yarn 1** (classic workspaces). Use workspace scripts from the repo root; do not assume `npm`/`pnpm` unless the user says otherwise.

## Project structure

- **`packages/excalidraw/`** — Main React component library published to npm as `@excalidraw/excalidraw`
- **`excalidraw-app/`** — Full-featured web application (excalidraw.com) that uses the library
- **`packages/`** — Shared packages: `@excalidraw/common`, `@excalidraw/element`, `@excalidraw/math`, `@excalidraw/utils`
- **`examples/`** — Integration examples (Next.js, browser script)

## Tech stack

- **Runtime:** Node `>=18` (see root `package.json` `engines`)
- **UI:** React (app on React 19; library supports peer range per `packages/excalidraw/package.json`)
- **Language:** TypeScript (strict)
- **App build:** Vite (`excalidraw-app`)
- **Library builds:** esbuild for packages
- **Tests:** Vitest at repo root; ESLint + Prettier for quality gates

## Development workflow

1. **Package development:** work in `packages/*` (especially `packages/excalidraw`) for editor features
2. **App development:** work in `excalidraw-app/` for hosting, collab, and app-only UI
3. **Testing:** run the checks in `.cursor/rules/testing.mdc` before commit; use `yarn test:update` when snapshots change intentionally
4. **Type safety:** `yarn test:typecheck`

## Development commands

```bash
yarn build           # Production build (excalidraw-app)
yarn start           # Dev server (Vite, from excalidraw-app)
yarn test            # Vitest (same as yarn test:app)
yarn test:typecheck  # TypeScript (tsc)
yarn test:code       # ESLint
yarn test:other      # Prettier check
yarn test:update     # Vitest with snapshot updates (non-watch)
yarn test:all        # typecheck + lint + prettier + tests
yarn fix             # Auto-fix formatting and lint
```

## Conventions

- **React:** for new/changed code prefer functional components + hooks and named exports; legacy class/default-export code exists — migrate when touching those files (see `.cursor/rules/conventions.mdc`)
- **TypeScript:** strict; avoid unnecessary `any` and `@ts-ignore`
- **Files:** PascalCase component files; kebab-case utilities; colocated `*.test.tsx` when tests add real signal
- **Editor state:** `actionManager.executeAction()` / existing patterns — not Redux/Zustand (see `.cursor/rules/architecture.mdc`)

## Do-not-touch / constraints

Do **not** edit these without explicit approval and full regression awareness (see `.cursor/rules/do-not-touch.mdc`):

- `packages/excalidraw/scene/renderer.ts`
- `packages/excalidraw/data/restore.ts`
- `packages/excalidraw/actions/manager.tsx`
- `packages/excalidraw/types.ts`

Security-sensitive areas: `.cursor/rules/security.mdc` (app shell + `packages/excalidraw/data/**`).

## Memory bank (`docs/memory/`)

Persistent context for agents and humans. **After substantive changes**, update the matching files so dates, versions, and commands stay true.

| File | Purpose | Update when |
|------|---------|-------------|
| [`docs/memory/projectbrief.md`](docs/memory/projectbrief.md) | Repo goals and scope | Scope or shipping model changes |
| [`docs/memory/productContext.md`](docs/memory/productContext.md) | Audiences and UX | User-facing behavior changes |
| [`docs/memory/activeContext.md`](docs/memory/activeContext.md) | Current focus, CI, hotspots | Workflow or CI gates change |
| [`docs/memory/techContext.md`](docs/memory/techContext.md) | Stack and versions | Dependencies or toolchain change |
| [`docs/memory/systemPatterns.md`](docs/memory/systemPatterns.md) | Architecture patterns | App/library boundaries change |
| [`docs/memory/decisionLog.md`](docs/memory/decisionLog.md) | ADR-style decisions | New technical decisions |
| [`docs/memory/progress.md`](docs/memory/progress.md) | Version snapshot | Package bumps or periodic refresh |

Related: [`docs/product/`](docs/product/), [`docs/technical/`](docs/technical/).

### How to maintain

1. Update memory bank in the **same branch/PR** as the code when the change affects documented areas.
2. Bump “as of” / snapshot dates when re-verifying; fix versions and commands when reality changes.
3. Keep cross-links valid if paths move.

## Architecture notes

### Package system

- Yarn workspaces: `excalidraw-app`, `packages/*`, `examples/*`
- Path aliases for tests and builds: see `vitest.config.mts`
- TypeScript strict configuration across packages
