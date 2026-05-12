# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

Single-file personal bookmark dashboard. No build step, no package manager, no framework. Open `index.html` directly in a browser.

## Architecture

Everything lives in `index.html`:

- **CSS** — inline `<style>` block using CSS custom properties (`--bg`, `--surface`, `--primary`, etc.) under `:root` and `[data-theme="dark"]`. All theming flows through these variables.
- **HTML** — static shell: header, `.controls` (search + add button), `#categories` nav, `#grid` main area, `#empty` fallback div, and a native `<dialog id="modal">` for add/edit.
- **JS** — inline `<script>` block, module-scoped globals, no bundler:
  - `bookmarks[]` — single source of truth; all mutations go through `save()` which writes JSON to `localStorage` under `"bookmarks_v1"`.
  - `load()` — seeds 8 sample bookmarks **only** when `localStorage.getItem("bookmarks_v1") === null` (not on empty array, to avoid re-seeding after the user deletes everything).
  - `renderGrid()` calls `getVisible()` which applies both `activeCategory` and `searchTerm` filters.
  - Cards are built with `buildCard(b)` using DOM API (not innerHTML) to avoid XSS.
  - SVG icons are stored as template-literal strings (`EDIT_SVG`, `DEL_SVG`, `SUN_SVG`, `MOON_SVG`) and injected via `innerHTML` only on trusted constant strings.
  - Theme is persisted under `"theme_v1"` and applied via `data-theme` attribute on `<html>`.

## How to develop

Open the file in a browser — no server required:

```
start c:\claude\projects\bookmarks\index.html   # Windows
```

Reload the page after edits. Use browser DevTools console to inspect `localStorage` state.

To reset to sample bookmarks: `localStorage.clear()` in DevTools console, then reload.

## Known environment issue

The GSD hooks in `~/.claude/hooks/` use PowerShell-style `& "node.exe" "script.js"` invocation but Claude Code shells out via bash, causing hook errors on every tool call. Hooks don't block Bash/Read execution but **do** block the `Write` tool (PreToolUse). To fix: change hook commands in `~/.claude/settings.json` to bash-compatible `node "/path/to/script.js"` syntax.
