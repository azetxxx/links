# Netlify: your domain → Scan2Buy (trycloudflare)

**Redirect-only** deploy: there is **no** `index.html` — only `public/_redirects`. No placeholder pages.

## Files

| File | Role |
|------|------|
| `public/_redirects` | **Generated** from `_redirects.template` — do not hand-edit; run `npm run tunnel:trycloudflare` from repo root (or `node …/cloudflared-tunnel-redirects.mjs --url …`) after each new trycloudflare hostname, then commit + deploy. |
| `public/_redirects.template` | Placeholder `__TUNNEL_ORIGIN__` — the single place to change redirect *shape*; tunnel base is filled in by the script. |
| `netlify.toml` | Publish directory = `public`. |

## When the trycloudflare hostname changes

1. **From repo root** (with the bridge on `PORT` in `server/.env`):

   ```bash
   npm run tunnel:trycloudflare
   ```

   The script runs `cloudflared`, detects the new `https://….trycloudflare.com`, and rewrites **`public/_redirects`** from **`_redirects.template`**.

2. **If you only have the new URL** (e.g. tunnel is already running in another window):

   ```bash
   node scripts/cloudflared-tunnel-redirects.mjs --url https://NEW.trycloudflare.com
   ```

3. **Commit + push (optional manual step):** from **`20_projects/scan2buy`**, `npm run tunnel:trycloudflare` **stages, commits, and `git push`** `public/_redirects` so Netlify can build on that push (if the site is linked to this branch). To only rewrite the file, no git: `npm run tunnel:trycloudflare:no-push` or set `SCAN2BUY_NETLIFY_GIT_PUSH=0`.

## Netlify + DNS

1. **Site** from Git; **publish** = `public` (see `netlify.toml`).
2. **Domains:** add **`scan2buy.az-studio.pro`** and **`s.az-studio.pro`**; in Netlify DNS use **CNAME** `scan2buy` and **`s`** → your **`<site>.netlify.app`** target.

## Behaviour

- **`https://s.az-studio.pro/`** (path `/`, including **`?p=…`** on label deep links) → **302** to tunnel `/`. Netlify **forwards the original query string** on 302 when the target URL does not already define one (do **not** put a fake `:query` placeholder in the target — that is not valid Netlify syntax and can drop `?p=`).
- **`https://s.az-studio.pro/*`** → **302** to tunnel **`/:splat`** (paths like **`/qr-generator/`**, **`/s/…`**).
- Plain **`/`** → same tunnel for any other hostname on this Netlify site (e.g. **`scan2buy.…`**).
