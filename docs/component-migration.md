# Web components review and migration notes (2026 guidance)

This repository splits the extension host code (`src/`) from the webview UI (`web/`). The webview modules behave like UI components and are compiled and packaged separately. This document summarizes the current webview modules and what would need to change if you want to migrate them into `src/`, with updated guidance aligned to 2026-era best practices.

## Current webview modules ("components")

| Module | Responsibility | Notes for migration |
| --- | --- | --- |
| `web/main.ts` | Entry point for the webview UI. | Depends on browser APIs and the webview runtime; cannot run in the extension host without a bundler + DOM/runtime boundary. |
| `web/graph.ts` | Graph rendering and interactions. | Uses DOM rendering; should stay in webview or be extracted into a shared, framework-agnostic rendering layer. |
| `web/dialog.ts` | Dialog flows in the UI. | Tied to webview UI state; migration requires refactoring UI to shared view models. |
| `web/contextMenu.ts` | Context menu logic for the webview. | Depends on DOM event handling; keep in webview. |
| `web/dropdown.ts` | Dropdown component behavior. | UI-only; can be extracted to a shared UI package if you introduce a bundler. |
| `web/findWidget.ts` | Find widget interactions. | UI-only; same migration constraints as other web modules. |
| `web/settingsWidget.ts` | Settings UI within the webview. | Relies on webview state and messaging; shared migration needs message abstraction. |
| `web/textFormatter.ts` | Formatting utilities for UI text. | Candidate for a shared module if you want to reuse in `src/`. |
| `web/utils.ts` | Webview utilities and helpers. | Some utilities could move to a shared package; others are browser-specific. |
| `web/global.d.ts` | Webview typings. | Would need to be split if `src/` becomes the single TS root. |

## Why `web/` is separate today

The extension uses distinct build pipelines for the extension host and the webview. The scripts below compile and package the webview assets independently, which is why the sources live in `web/` and not `src/`.

```
"compile-src": "tsc -p ./src && node ./.vscode/package-src.js",
"compile-web": "tsc -p ./web && node ./.vscode/package-web.js",
"compile-web-debug": "tsc -p ./web && node ./.vscode/package-web.js debug",
```

## 2026-recommended implementation for a migration

If you plan to migrate or reorganize webview components in 2026, the most correct approach is to **treat the webview as a separate front-end build target** and use a modern bundler with strict runtime boundaries:

1. **Adopt a bundler for the webview** (esbuild or Rollup)
   - Build the webview into a single JS/CSS bundle and emit to `media/`.
   - Replace the current `package-web.js` concatenation/minification step.
   - Keep the extension host build (`src/`) separate and lean.

2. **Create a shared runtime-agnostic layer**
   - Extract pure utility modules (formatters, parsing, mapping) into `src/shared/` or `shared/`.
   - Use path aliases (TS `paths`) and include shared sources in both build targets.
   - Avoid direct DOM usage in shared modules.

3. **Introduce explicit messaging contracts**
   - Define a small `webview-protocol.ts` with request/response types.
   - Use compile-time checks to prevent host/webview API drift.

4. **Keep `src/` and `web/` as separate roots**
   - Even if you move files under `src/web/`, keep a separate build config for the webview.
   - This keeps Node/Electron host dependencies isolated from browser-only code.

## What would break if you directly move everything into `src/`

- The extension host build would try to compile webview-only code (DOM globals, `document`, etc.).
- Packaging scripts assume webview output is in `media/` and are driven by `web/` inputs.
- The current `compile-web` task would no longer find `web/tsconfig.json`.

If you want, I can propose an exact migration plan (esbuild or Rollup) once you confirm the preferred tooling and target structure.
