# Bookmark Dashboard

A personal bookmark manager that runs entirely in the browser — no server, no install, no account needed.

## Usage

Open `index.html` in any modern browser. That's it.

## Features

- **Add / edit / delete** bookmarks with a title, URL, category, and optional notes
- **Grid layout** with site favicons, category badges, and clickable title links
- **Live search** — filters by title or category as you type
- **Category filters** — All, Learning, Personal, Books, Shopping, Entertainment
- **Dark mode** — toggle in the top-right corner; preference is saved
- **Persists across reloads** via `localStorage` (key: `bookmarks_v1`)
- **Sample bookmarks** seeded on first launch (won't re-appear after you delete them)
- **Responsive** — single-column layout on narrow/mobile screens

## Reset

To wipe all bookmarks and reload the sample set, run this in the browser console:

```js
localStorage.clear(); location.reload();
```

## Tech

Single HTML file — inline CSS and vanilla JS, zero dependencies, zero build step. Works offline.
