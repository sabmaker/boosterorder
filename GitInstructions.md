# Publishing Booster Order to GitHub Pages

Goal: get `BoosterOrder.html` to a public URL so volunteers can scan a QR code,
open it on their phone, and "Add to Home Screen" — no app store, no login, no
file downloads.

---

## Context for a fresh Claude session

- **Project folder:** `E:\Code\BoosterOrder\`
- **The app:** `index.html` — a single self-contained file (HTML + CSS + JS
  inline, zero dependencies, no build step). Concession-stand order taker:
  item tiles with +/- quantity, running total, Clear, light/dark toggle,
  sort by Item or Category, collapsible categories with Collapse/Expand All,
  and a Summary overlay.
- **Item list** lives in a clearly-marked `const ITEMS = [...]` block at the top
  of the `<script>` tag. Fields: `name`, `category`, `price`, `symbol`.
  Colored variants use a base emoji plus a color dot (e.g. `"🍬🔴"`).
- **State persisted to localStorage:** theme, sort mode, collapsed categories.
  (Quantities are intentionally NOT saved — each order starts clean.)
- **PWA / home screen support is already done.** Apple meta tags are in
  `<head>`; the touch icon and the web manifest are generated at runtime in an
  `installMeta()` IIFE near the bottom of the script (canvas-drawn PNG + blob
  URL manifest) so the file stays single-file and portable. To restyle the
  home screen icon, edit `APP_ICON` / `APP_ICON_BG` just above that function.
- **iOS tap-zoom** is suppressed via `touch-action: manipulation` on `*`.
  Do not "fix" this with viewport `user-scalable=no` — Safari ignores that.

---

## The file is already named `index.html`

GitHub Pages serves `index.html` automatically at the folder URL, giving you
`https://USERNAME.github.io/boosterorder/` rather than a longer path. Shorter
URL = simpler QR code. **Do not rename it** — just commit it as-is.

---

## Option A — Web UI only (no git installed)

1. Go to <https://github.com> and sign in.
2. **New repository**
   - Name: `boosterorder`
   - Visibility: **Public** (required for free GitHub Pages)
   - Check "Add a README file"
   - Create repository
3. Click **Add file → Upload files**, drag in `index.html`, and commit.
4. **Settings → Pages** (left sidebar)
   - Source: **Deploy from a branch**
   - Branch: `main`, folder: `/ (root)` → Save
5. Wait 1-2 minutes. The URL appears at the top of that same Pages screen:
   `https://USERNAME.github.io/boosterorder/`

**To update later:** open `index.html` in the repo → pencil icon → paste new
contents → Commit. Live in about a minute.

---

## Option B — Git command line

```bash
cd /e/Code/BoosterOrder
git init
git branch -M main
git add index.html GitInstructions.md
git commit -m "Add Booster Order concession app"
git remote add origin https://github.com/USERNAME/boosterorder.git
git push -u origin main
```

Then enable Pages: **Settings → Pages → Deploy from a branch → main → / (root)**.

Updating after edits:

```bash
cd /e/Code/BoosterOrder
git add index.html
git commit -m "Update item list and prices"
git push
```

---

## Gotchas

- **Repo must be Public.** Private repos need a paid plan for Pages.
- **Deploys are not instant.** Allow 1-2 minutes after pushing.
- **Browser caching.** After an update, phones may show the old version. Hard
  refresh, or bump a version comment in the file. If this becomes a recurring
  annoyance, ask Claude to add a cache-busting query string to the QR URL
  (e.g. `?v=2`).
- **Everything is public.** Don't put anything sensitive in the file — prices
  and emoji are fine, obviously.

---

## After it's live

1. **Generate a QR code** for the URL. Print it on a card at the stand.
   Ask Claude to generate one, or use any QR site — the QR just encodes a URL.
2. **Tell volunteers to install it:**
   - *iPhone (Safari):* Share button → Add to Home Screen
   - *Android (Chrome):* ⋮ menu → Add to Home screen / Install app

   Installed, it launches with a 🍔 icon, no address bar, locked to portrait.
   **This only works over the hosted URL (https), not from a local file.**
3. **Optional:** a service worker for true offline use if the stand has bad
   signal. Note this breaks the strict single-file setup — it needs a second
   file at the site root.

---

## Alternatives to GitHub Pages

| Host | Setup | Catch |
|---|---|---|
| **GitHub Pages** | Repo + toggle a setting | Permanent, free. Recommended. |
| **Cloudflare Pages** | Drag-and-drop upload | Free, needs a Cloudflare account |
| **Netlify** | Drag-and-drop | Anonymous "Drop" deploys expire unless claimed |
| **Dropbox** | — | **Avoid.** Won't render HTML, prompts sign-in, forces a file download |
