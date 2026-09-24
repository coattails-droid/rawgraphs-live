# RAWGraphs 2.0 — live static build

Static production build of [rawgraphs/rawgraphs-app](https://github.com/rawgraphs/rawgraphs-app),
the open-source D3.js data-visualization web app (~9k stars, Apache-2.0).

Live at: https://coattails-droid.github.io/rawgraphs-live/

## Build provenance

- **Upstream repo:** rawgraphs/rawgraphs-app
- **Upstream commit:** b7b2909111cc029ccf418dc3e7d079e0f4c50d6f (2025-01-28)
- **Build command:** `cd ~/workspace/rawgraphs-build && NODE_OPTIONS=--openssl-legacy-provider PUBLIC_URL=/rawgraphs-live/ GENERATE_SOURCEMAP=false DISABLE_ESLINT_PLUGIN=true node node_modules/react-scripts/bin/react-scripts.js build`
- **Build date:** 2026-09-24
- **License:** Apache License 2.0 — see `LICENSE` (copied verbatim from upstream)

## Build notes

- Upstream uses `yarn install && yarn build` (Create React App 4 / webpack 4). This mirror
  was built with `npm ci` from a `package-lock.json` mechanically converted from the
  upstream `yarn.lock` (exact pinned versions, incl. `react-data-grid@7.0.0-canary.16`),
  with `NODE_OPTIONS=--openssl-legacy-provider` (webpack 4 needs the legacy OpenSSL
  provider on modern Node) and `PUBLIC_URL=/rawgraphs-live/` so asset URLs resolve
  under the GitHub Pages subpath. `GENERATE_SOURCEMAP=false` (no `.map` files — cuts
  webpack's memory use; the build container has ~8 GB shared with other jobs) and
  `DISABLE_ESLINT_PLUGIN=true` (skips the lint pass to reduce build CPU).
  Terser's on-disk cache was disabled (`cache: false`)
  because its `lchown` fails in the build container; minification itself is unchanged.
- This repo contains **only the static build output** (`build/` contents at root), the
  upstream `LICENSE`, and this README. No `node_modules`, no build tooling.
- All data is processed in-browser; the app needs no server-side storage or operations
  (per the upstream README). No client-side routing, so no `404.html` fallback is needed.
- Static mirror — for issues with the app itself, please file them upstream at
  rawgraphs/rawgraphs-app.
