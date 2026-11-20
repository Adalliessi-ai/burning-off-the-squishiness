# Deploying to GitHub Pages

Repo: `Adalliessi-ai/burning-off-the-squishiness`
Live at: `https://adalliessi-ai.github.io/burning-off-the-squishiness/`

## Files — all go in the repo root

| File | What it does |
|---|---|
| `index.html` | The whole app. Font is embedded, so it works offline. |
| `manifest.webmanifest` | Makes it installable: name, icons, standalone mode, colours. |
| `sw.js` | Service worker. Caches everything so the app opens with no signal. |
| `icon-192.png` | Android home screen |
| `icon-512.png` | Android splash + store listing |
| `icon-512-maskable.png` | Android adaptive icon (safe zone respected) |
| `apple-touch-icon.png` | iPhone home screen |
| `favicon-32.png` | Browser tab |
| `.nojekyll` | Stops GitHub Pages running Jekyll over the files. Keep it. |

## First deploy

1. Upload all nine files to the repo root (replacing the old `index.html`).
2. Settings → Pages → Source: `main` branch, `/ (root)`.
3. Wait ~60 seconds for the build.

## Installing on your iPhone

1. Open the URL in **Safari** (Chrome on iOS cannot install PWAs).
2. Share → **Add to Home Screen**.
3. **Delete the old home screen icon first** — the old one has no service worker
   and iOS will keep serving the stale version.

## Every time you update the app

Two steps, and the second one is the one people forget:

1. Replace `index.html`.
2. Open `sw.js` and bump the version: `squish-v1` → `squish-v2` → `v3`...

Without step 2 the service worker serves the cached old app forever. With it,
the app detects the new version, swaps it in, and reloads itself once.

## Offline behaviour

- **Pages**: fetched fresh when you have signal, served from cache when you don't.
- **Icons, manifest**: cache first.
- **The Coach tab**: needs internet — it calls the Claude API. Everything else
  (warm-ups, sessions, ticking, Body tab, roadmap) works fully offline.

## If you buy Lufga

1. Drop `Lufga.woff2` in the repo root.
2. In `index.html`, find `/* ── FONT` and uncomment the `@font-face` block above it.
3. Add `'./Lufga.woff2'` to the `ASSETS` array in `sw.js`.
4. Bump the cache version.

`--font` already lists Lufga ahead of Outfit, so it takes over with no other changes.
