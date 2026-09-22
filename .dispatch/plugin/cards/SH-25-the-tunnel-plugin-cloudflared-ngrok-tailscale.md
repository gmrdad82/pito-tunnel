# SH-25 — The tunnel plugin: cloudflared, ngrok, Tailscale

Wave: 1 · Run: single · Depends on: none · Complexity: ***
Branch: split-sh-25 · Base: main

## What
One plugin, three providers, on `pito:remote`: a page in Plugins with Start, Stop, the status and the public URL; each provider a program the plugin may start.

## How
1. `plugins/tunnel-common`: the state machine and the three output scanners.
2. cloudflared, tailscale, ngrok providers; manifests with `exec:<program>`.
3. Build to `wasm32-wasip2`; validate; list.

## Guards
- Do not touch: `wit/`, `templates/`, `themes/`, the other plugins.
- Gate: the project's gate
- Accept: the three plugins build and validate; the scanners' tests pin masked real fixtures.
