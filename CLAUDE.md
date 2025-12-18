# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
pnpm install          # Install dependencies
pnpm dev              # Start all dev watchers (extension + webview + packages)
pnpm build            # Build all packages
pnpm lint             # Lint all packages
pnpm format           # Format code with prettier
pnpm clean            # Clean build artifacts and node_modules
```

Debug: Press F5 in VS Code to launch extension host, then run "Show WebView" command.

## Architecture

Turborepo monorepo with pnpm workspaces:

```
apps/vscode-extension  → VS Code extension entry (webpack bundle)
packages/core          → Shared business logic (@template/core)
packages/ui            → React components + Tailwind (@template/ui)
packages/webview       → WebView React app (@template/webview)
```

**Dependency flow:** `vscode-extension` → `webview` → `ui` + `core`

**Extension-WebView communication:**
- Extension: `panel.webview.postMessage()` / `onDidReceiveMessage`
- WebView: `useVSCode()` hook from `VSCodeProvider` context for `postMessage/setState/getState`

## Tailwind

Use VS Code CSS variables via semantic tokens (defined in `packages/ui/tailwind.config.js`):

```
bg-vscode-background, text-vscode-foreground
bg-vscode-button-background, text-vscode-button-foreground
bg-vscode-input-background, border-vscode-input-border
```
