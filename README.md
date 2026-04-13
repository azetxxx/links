# Netlify: your domain → Scan2Buy (trycloudflare)

**Redirect-only** deploy: there is **no** `index.html` — only `public/_redirects`. No placeholder pages.

## Files

| File | Role |
|------|------|
| `public/_redirects` | All rules (tunnel URL appears in several lines — keep them in sync). |
| `netlify.toml` | Publish directory = `public`. |

## Before / after tunnel URL changes

Edit **`public/_redirects`**: replace every `https://milwaukee-scientific-old-eagle.trycloudflare.com` (or your current tunnel) with the new base URL. Commit and push.

## Netlify + DNS

1. **Site** from Git; **publish** = `public` (see `netlify.toml`).
2. **Domains:** add **`scan2buy.az-studio.pro`** and **`s.az-studio.pro`**; in Netlify DNS use **CNAME** `scan2buy` and **`s`** → your **`<site>.netlify.app`** target.

## Behaviour

- **`https://s.az-studio.pro/*`** → **302** to the same path on the tunnel (`:splat`).
- **`https://scan2buy.az-studio.pro/`** → **302** to tunnel `/` (via the `/` rule).
- **`https://scan2buy.az-studio.pro/go`** → **302** to tunnel `/` (explicit short path; optional if you only use `s`).
