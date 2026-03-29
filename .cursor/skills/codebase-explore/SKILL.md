# Skill: Codebase Explorer

## When to use

When you need to understand an unfamiliar area of the codebase.
Triggered by: "explore", "investigate", "how does X work?"

## Inputs

- Area of interest (module, feature, file pattern)

## Steps

1. Identify relevant directory/files using @folder or @codebase — for this monorepo, typical roots are **`packages/excalidraw/`** (editor library), **`excalidraw-app/`** (hosted app), and **`packages/{common,element,math,utils}/`** (shared).
2. Read `README.md` / package docs and `package.json` `scripts` in the touched package for build and entry hints.
3. Map the key files and their responsibilities
4. Trace data flow: for user-driven editor behavior, follow **`actionManager.executeAction()`** and related actions; for drawing, follow **scene updates → `renderStaticScene` / `renderInteractiveScene` → canvas** (not React DOM for the canvas surface).
5. Identify dependencies (imports from other packages)
6. Document findings in a summary

## Outputs

- Summary: purpose, key files, data flow, dependencies
- List of related files for deeper investigation

## Safety

- READ-ONLY — do not modify any files during exploration
- Verify findings against actual code, not assumptions
