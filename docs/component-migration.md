# Web components review and migration notes

This repository splits the extension host code (`src/`) from the webview UI (`web/`). The webview modules behave like UI components and are compiled and packaged separately. This document summarizes the current webview modules and what would need to change if you want to migrate them into `src/`.

## Current webview modules ("components")

| Module | Responsibility | Notes for migration |
| --- | --- | --- |
| `web/main.ts` | Entry point for the webview UI. | Depends on browser APIs and the webview runtime; cannot run in the extension host without a bundling + DOM shim strategy. |
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

## Migration options (recommended approach)

If you want to "move" web components under `src/`, there are two viable strategies:

1. **Create a shared package for cross-runtime code**
   - Keep UI-only modules in `web/`.
   - Extract pure utilities (e.g., `textFormatter`, parts of `utils`) into `src/shared/` or a new `shared/` folder.
   - Update both `tsconfig.json` files to include shared sources.

2. **Adopt a bundler and unify the TS project**
   - Move webview code under `src/web/` but keep a separate build target.
   - Replace `compile-web` with a bundler step (esbuild/rollup) that outputs the webview assets to `media/`.
   - This reduces duplication but still keeps different runtime targets.

## What would break if you directly move everything into `src/`

- The extension host build would try to compile webview-only code (DOM globals, `document`, etc.).
- Packaging scripts assume webview output is in `media/` and are driven by `web/` inputs.
- The current `compile-web` task would no longer find `web/tsconfig.json`.

If you want, I can propose an exact migration plan once you confirm which option you want.
