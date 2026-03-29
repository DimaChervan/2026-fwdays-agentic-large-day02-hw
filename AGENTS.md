# AGENTS.md

## Project Structure

Excalidraw is a **monorepo** with a clear separation between the core library and the application:

- **`packages/excalidraw/`** - Main React component library published to npm as `@excalidraw/excalidraw`
- **`excalidraw-app/`** - Full-featured web application (excalidraw.com) that uses the library
- **`packages/`** - Core packages: `@excalidraw/common`, `@excalidraw/element`, `@excalidraw/math`, `@excalidraw/utils`
- **`examples/`** - Integration examples (NextJS, browser script)

## Development Workflow

1. **Package Development**: Work in `packages/*` for editor features
2. **App Development**: Work in `excalidraw-app/` for app-specific features
3. **Testing**: Always run `yarn test:update` before committing
4. **Type Safety**: Use `yarn test:typecheck` to verify TypeScript

## Development Commands


```bash
yarn test:typecheck  # TypeScript type checking
yarn test:update     # Run all tests (with snapshot updates)
yarn fix             # Auto-fix formatting and linting issues
```

## Memory bank (`docs/memory/`)

The **memory bank** is persistent project context for agents and humans. **After substantive changes, update the relevant files** so dates, versions, commands, and descriptions stay consistent with the repo.

### Files

| File | Purpose | Update when |
|------|---------|-------------|
| [`docs/memory/projectbrief.md`](docs/memory/projectbrief.md) | What the repo is, goals, scope | Goals, scope, or what ships where changes |
| [`docs/memory/productContext.md`](docs/memory/productContext.md) | Audiences, UX flows | User-facing behavior or major flows change |
| [`docs/memory/activeContext.md`](docs/memory/activeContext.md) | Current focus, CI expectations, hotspots | Workflow, CI gates, or maintenance focus changes |
| [`docs/memory/techContext.md`](docs/memory/techContext.md) | Stack, tooling, versions | Dependencies, Node/Yarn, build or test setup changes |
| [`docs/memory/systemPatterns.md`](docs/memory/systemPatterns.md) | Architecture and code patterns | Boundaries between app/packages or major patterns change |
| [`docs/memory/decisionLog.md`](docs/memory/decisionLog.md) | Decisions (context → decision → consequences) | New architectural or tooling decisions |
| [`docs/memory/progress.md`](docs/memory/progress.md) | Version snapshot and inventory | Package or toolchain versions change; refresh periodically |

Related docs: [`docs/product/`](docs/product/), [`docs/technical/`](docs/technical/)—keep them in sync when product or architecture documentation is affected.

### How to maintain

1. **Same branch / PR** — When work touches something described in the memory bank, edit the matching file(s) together with the code.
2. **Facts and dates** — Bump “as of” / snapshot dates when re-verifying; fix paths, commands, and version numbers when reality changes.
3. **Cross-links** — These files link to each other and to `docs/product/` and `docs/technical/`; update links if paths or structure move.

## Architecture Notes

### Package System

- Uses Yarn workspaces for monorepo management
- Internal packages use path aliases (see `vitest.config.mts`)
- Build system uses esbuild for packages, Vite for the app
- TypeScript throughout with strict configuration