# KiPad Studio — static build

A self-contained, single-file PCB editor (`index.html` — no build step, no dependencies to install).

## Deploy on Vercel

**Option A — Vercel dashboard**
1. Unzip this folder.
2. Go to vercel.com → **Add New… → Project**.
3. Choose **"Deploy without Git"** / drag-and-drop the unzipped folder onto the import screen.
4. Framework preset: **Other** (static). Leave build command empty, output directory `.`.
5. Deploy.

**Option B — Vercel CLI**
```bash
npm i -g vercel
cd kipad-studio-vercel
vercel --prod
```

**Option C — GitHub**
Push this folder to a GitHub repo, then import that repo in Vercel. Any push to the connected branch will redeploy automatically.

## What works standalone vs. only inside Claude

This file was originally built to run inside a Claude.ai artifact, which offers a couple of optional
platform features (`window.claude.use(...)`). Outside Claude those aren't defined, and the app
already checks for that and falls back gracefully:

- **Board editing, templates, footprint library, DRC, localStorage save** — fully work, no changes needed.
- **GitHub push/pull (token mode)** — fully works: it's plain `fetch()` calls to `api.github.com`, nothing Claude-specific.
- **Export .kicad_pcb / .kicad_pro** — on Vercel this automatically uses a normal browser download
  (a Blob + `<a download>` link), so it works exactly like any other website's "Download" button — no
  extension restrictions, no zip wrapping needed (that workaround only applies inside the Claude viewer).
- **GitHub device-login relay** — unchanged; still needs the small serverless relay (code downloadable from
  inside the app) if you want app-style sign-in instead of a pasted token.

No environment variables or server-side code are required — it's 100% static.
