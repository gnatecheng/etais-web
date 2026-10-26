**English** | [中文](README.zh-CN.md)

# etais-web

Source for **[etais.dev](https://etais.dev)** — the Etai Apps marketing homepage (Cloudflare Worker **`etai`** + static assets in [`site/`](site/)).

APK files are **not** built or hosted in this repository. Download buttons and QR codes point at **https://dl.etais.dev** (configured in [`scripts/site-download-config.mjs`](scripts/site-download-config.mjs)). After publishing a new APK to R2, update that file, run `python3 scripts/generate-download-qr-svgs.py` (requires `qrcode`), then `npm run build`.

## Layout

| Path | Purpose |
| --- | --- |
| [`site/`](site/) | Static HTML, CSS, JS, images (served via Workers Assets) |
| [`worker/`](worker/) | Worker entry (`index.js`) — canonical host redirects, optional D1 access logging |
| [`wrangler.jsonc`](wrangler.jsonc) | Worker name **`etai`**, assets binding, D1 **`ACCESS_LOG`** → `etai-access-log` |
| [`migrations/`](migrations/) | D1 schema for page visit logs |
| [`scripts/`](scripts/) | Site generators (HTML/SEO/download panels), helpers, and local verification |

## Prerequisites

- **Node.js 20+** and npm
- **Cloudflare account** with Wrangler CLI (installed via this repo’s devDependencies)
- Optional: Python 3 with `qrcode` for QR SVG generation; `playwright` browsers for screenshot/render scripts (`npx playwright install chromium`)

## Install

```bash
npm install
```

## Build generated HTML

Regenerates app pages, privacy pages, `/en/` homepage sections, and SEO snippets after you edit shared config or templates:

```bash
npm run build
# equivalent: node scripts/build-site.mjs
```

## Local preview

```bash
npm run dev
# or: npx wrangler dev
```

You can also serve `site/` with any static file server (Worker-only behavior such as redirects and access logging requires Wrangler).

## Deploy

Production deploy is typically **Cloudflare Workers Builds** (Git integration on `main`), using [`wrangler.jsonc`](wrangler.jsonc) at the repo root.

Manual deploy:

```bash
npm run deploy
```

### D1 access log

Apply the schema once per database (remote example):

```bash
npx wrangler d1 execute etai-access-log --remote --file=./migrations/0001_access_log.sql
```

The Worker skips logging when the `ACCESS_LOG` binding is unavailable locally.

## Cache busting

Long-lived cache headers apply to `/css/*` and `/js/*` ([`site/_headers`](site/_headers)). After editing CSS or JS, bump the `?v=` query on script/style URLs in [`site/index.html`](site/index.html) and [`site/404.html`](site/404.html).

## Featured apps

| App | Repository |
| --- | --- |
| C Week | [gnatecheng/c-week](https://github.com/gnatecheng/c-week) |
| Easy Ledger | [gnatecheng/easy-ledger](https://github.com/gnatecheng/easy-ledger) |
| Group Matters | [gnatecheng/group-matters](https://github.com/gnatecheng/group-matters) |

## License

MIT — see [LICENSE](LICENSE).
