# GENESIS — The AI Atlas

An interactive atlas of AI, from first principles to generative systems: 28 chapters
and 26 expanded 3D scenes. Opens with the foundations — what AI is, machine learning,
neural networks, deep learning, matrix multiplication — then follows the generative
journey through training, transformers, reinforcement learning, agents, retrieval,
graphs and multimodal generation.

**Live:** https://genesis-atlas-puce.vercel.app

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

## Deploying

Pushes to `main` deploy to production automatically via Vercel.

Asset filenames are not content-hashed, so `vercel.json` sets
`Cache-Control: public, max-age=0, must-revalidate` on the CSS and JS. Leave that
in place, or visitors will be served stale JavaScript after an update.
