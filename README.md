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
3. **Domain:** Site settings → **Domain management** → add custom hostnames. Netlify usually asks for **CNAME** records in Netlify DNS (exact target from the UI, e.g. **`<your-site>.netlify.app`**):

   | Hostname | DNS label (in Netlify DNS) |
   |----------|----------------------------|
   | `scan2buy.az-studio.pro` | **CNAME** name `scan2buy` → Netlify target |
   | **`s.az-studio.pro`** (alias) | **CNAME** name **`s`** → **same** Netlify target |

   Both names serve the **same** deploy and the **same** `public/_redirects` rules.

   **Conflict check:** one hostname → one target. If a label was a **CNAME to `…cfargotunnel.com`**, switch it to Netlify for this redirect site. The real Scan2Buy app URL stays in `_redirects`.

## After each tunnel restart

Quick tunnels get a new hostname. Update the URL in `public/_redirects`, commit, push. Redeploy finishes in seconds.

## Optional: redirect the site root

Uncomment the `/` line in `_redirects` and set the tunnel URL so **`https://scan2buy.az-studio.pro/`** goes straight to Scan2Buy (then `index.html` is not used for `/`).

## Paths (your setup)

Same path on **either** hostname:

- **`https://scan2buy.az-studio.pro/go`**
- **`https://s.az-studio.pro/go`** — shorter host for bookmarks / QR.

Both → tunnel URL from `_redirects` (default: Scan2Buy app root). Change the right-hand side to **`…/qr-generator/`** if you want `/go` to open labels only.
