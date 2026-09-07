# GENESIS — The AI Atlas

An interactive atlas of AI, from first principles to generative systems: 28 chapters
and 26 expanded 3D scenes. Opens with the foundations — what AI is, machine learning,
neural networks, deep learning, matrix multiplication — then follows the generative
journey through training, transformers, reinforcement learning, agents, retrieval,
graphs and multimodal generation.

**Live:** https://atlas.sayangupta.in

## Structure

A dependency-free static site. No build step, no bundler, no backend.

| File | Role |
| --- | --- |
| `index.html` | Page shell, header, chapter rail, reading-room markup |
| `style.css` | All styling and the design tokens (`--bg`, `--mint`, `--violet`, `--amber`) |
| `app.js` | Entry point (ES module): canvas scene, camera, chapter routing |
| `foundations.js` | The "start here" foundations chapters and their scenes |
| `chapters.js` | Chapter and micro-scene content data |
| `expansion.js` | The "inside this subsystem" expanded views |
| `favicon.svg` | Brand mark |
| `og.jpg` | 1200x630 social share card |
| `og-card.html` | Source for `og.jpg` (not deployed; see below) |
| `vercel.json` | Clean URLs and revalidating cache headers |

The only external dependency is the DM Sans stylesheet from Google Fonts,
imported at the top of `style.css`.

## Local development

Any static file server works, but it must be served over HTTP — `app.js` is an
ES module, so opening `index.html` from the filesystem will fail on CORS.

```bash
python3 -m http.server 8791
```

Then open http://localhost:8791.

## The social share card

`og.jpg` is the Open Graph / Twitter card image, referenced by absolute URL in the
`<head>`. It is generated from `og-card.html`, which is excluded from deploys via
`.vercelignore`. To regenerate after a copy change:

```bash
"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" \
  --headless --disable-gpu --hide-scrollbars --force-device-scale-factor=1 \
  --window-size=1200,630 --virtual-time-budget=8000 \
  --screenshot=og.png og-card.html
```

Then convert `og.png` to `og.jpg` — the JPEG is ~66 KB against ~360 KB for the PNG,
which matters because WhatsApp will not fetch a preview image much over 300 KB.

Note that `og:url`, `og:image`, `twitter:image` and the canonical link are absolute
URLs pointing at `atlas.sayangupta.in`. If the domain changes again, update all four
in `index.html` or the previews will keep pointing at the old host.

## Deploying

Pushes to `main` deploy to production automatically via Vercel.

`atlas.sayangupta.in` is the canonical host. The three stable `*.vercel.app`
aliases 308 to it, configured as host-matched `redirects` in `vercel.json`, so the
site answers on one URL and search engines consolidate on it. Per-deployment
URLs (`genesis-atlas-<hash>-*.vercel.app`) and branch previews are deliberately
left alone, so a preview can still be checked before it is promoted.

Asset filenames are not content-hashed, so `vercel.json` sets
`Cache-Control: public, max-age=0, must-revalidate` on the CSS and JS. Leave that
in place, or visitors will be served stale JavaScript after an update.
