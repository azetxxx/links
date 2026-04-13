# Netlify: short link on your domain → Cloudflare tunnel (Scan2Buy)

Minimal static site: one **HTTP redirect** you can update whenever your `trycloudflare.com` URL changes. No Bitly, no ads.

## What’s in the repo

| File | Role |
|------|------|
| `public/_redirects` | Netlify redirect rules (`/go` → your tunnel URL). |
| `public/index.html` | Tiny homepage at `/` (optional; you can redirect `/` too — see comments in `_redirects`). |
| `netlify.toml` | Publish directory = `public`. |

## Before first deploy

1. Open `public/_redirects`.
2. Replace `https://REPLACE-ME.trycloudflare.com/` with your real tunnel base URL (no trailing issues: one slash before path is fine).
3. Commit and push.

## Netlify setup

1. **New site** from Git → pick the repo (this folder can live inside the same repo; set **Base directory** in Netlify to `deploy/netlify-redirect` if the repo root is not only this site).
2. **Build settings:** leave default; there is no build command. **Publish directory:** `public` if the Netlify **base directory** is `deploy/netlify-redirect`; if the site root **is** this folder, publish is still `public` (already set in `netlify.toml`).
3. **Domain:** Site settings → **Domain management** → add **`az-studio.pro`** (and `www` if you want). Netlify will show required DNS; your DNS is already at Netlify for `az-studio.pro` — add the records it asks for (often **ALIAS/ANAME** apex to the Netlify load balancer, or follow their wizard).

## After each tunnel restart

Quick tunnels get a new hostname. Update the URL in `public/_redirects`, commit, push. Redeploy finishes in seconds.

## Optional: redirect the whole apex

Uncomment the `/` line in `_redirects` and set the same tunnel URL so `https://az-studio.pro/` goes straight to Scan2Buy (then the small `index.html` is never used for `/`).

## Paths

- **`https://az-studio.pro/go`** → your Scan2Buy tunnel root (adjust in `_redirects` if you want `/qr-generator/` instead).
