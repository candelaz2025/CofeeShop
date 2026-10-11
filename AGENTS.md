# Casa Olive — working notes for agents

## What this project is
A single-file interactive 3D architectural visual tour. There is **no build
step, no backend, no database and no environment variables**: `casa-olive-3d/index.html`
contains all markup, CSS and the Three.js module script.

## Non-obvious setup facts
- The **site root is `casa-olive-3d/`**, not the repository root. The GitHub
  Pages workflow (`.github/workflows/pages.yml`) uploads exactly that directory,
  so any static server must be rooted there for `/` to resolve to `index.html`.
- Runtime dependencies come from **public CDNs**, not `node_modules`:
  - an `<script type="importmap">` maps `three` / `three/addons/` to
    `cdn.jsdelivr.net`
  - Google Fonts (DM Sans, Playfair Display)
  Because of the import map, do **not** introduce a bundler/dev server that
  rewrites module imports (e.g. Vite) — it cannot resolve the bare `three`
  specifier and would break the page. Plain static serving is required.
- No credentials are needed for anything; there is nothing to configure.

## Running it
```
docker compose -f docker-compose.base44.yml up -d --build
```
`docker-compose.base44.yml` runs `live-server` (fetched with `npx`, cached on a
named volume) over the bind-mounted `casa-olive-3d/` directory on host port
3000. Edits to `index.html` are picked up via full page reload.

## How to verify it works
```
curl -s http://localhost:3000/ | grep -q "CASA OLIVE" && echo ok
```
Then in the browser: the canvas renders the villa, the bottom chip row switches
between the 8 views, the ☀/☾ segment toggles the lighting preset, and
`Design Details` / `Drawings & Downloads` open their panels.

## Sandbox overrides
None. The site is plain static HTML, so it needs no host/origin allowlist and no
`BASE44_PREVIEW_MODE`-gated behavior.
