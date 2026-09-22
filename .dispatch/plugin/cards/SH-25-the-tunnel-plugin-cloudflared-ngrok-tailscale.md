# SH-25 — The tunnel plugin: cloudflared, ngrok, Tailscale on `pito:remote`, one plugin, three providers

Wave: 1 · Run: single · Depends on: pito-work/WK-20 · Complexity: **
Branch: split-sh-25 · Base: main

## What
Three first-party plugins in `pito-plugins/plugins/`: `tunnel-cloudflared`, `tunnel-tailscale`, `tunnel-ngrok`, sharing one library crate `plugins/tunnel-common` (the state machine, the page tree, the URL scanners, the provider trait), each with its `plugin.toml` (`hosts = ["work", "pigeon", "studio"]`, capabilities `remote:tunnel`, `exec:<program>`, `notify`, and `net:127.0.0.1` for ngrok only, slot `settings`), each built to `wasm32-wasip2`, listed in `index.json` with a release asset and its SHA-256, and documented in `docs/modules.md`'s worked example (SH-24). Tests on `tunnel-common` pin the scanners and the state machine; the rig story proves cloudflared end to end on the Work desk.

## How
1. **`plugins/tunnel-common`** (a Rust library, `wit_bindgen::generate!` over `../../wit/pito-host/0.1` for the `remote`, `ui`, `page`, `notify`, `log` and, for ngrok, `net` imports — the generated bindings live in each plu
2. **cloudflared**: `PROGRAM = "cloudflared"`, args `["tunnel", "--url", format!("http://{listen}")]`; `scan` matches `https://[a-z0-9-]+\.trycloudflare\.com` on both streams (the docs say "printed in the terminal", the str
3. **tailscale**: `PROGRAM = "tailscale"`; start = `["funnel", "--bg", listen_port]` (or `["serve", "--bg", listen_port]` when `tailnet_only`), then a second exec `["funnel", "status", "--json"]` (or `serve status --json`)
4. **ngrok**: `PROGRAM = "ngrok"`; args `["http", listen]` plus `["--url", reserved]` when set; the URL comes from `fetch("http://127.0.0.1:4040/api/tunnels")` (SH-11's `net`, granted `net:127.0.0.1`) parsed for `public_url
5. **Manifests** (`plugins/tunnel-<provider>/plugin.toml`): `id = "pito/tunnel-<provider>"`, `kind = "plugin"`, `hosts = ["work", "pigeon", "studio"]`, `api = "0.1"`, `capabilities = ["remote:tunnel", "exec:<program>", "not
6. **Build and list**: each plugin `cargo build --release --target wasm32-wasip2 -p tunnel-<provider>`; `sha256sum` of each `.wasm`; an `index.json` entry per plugin with `asset_url` at the repo's release for this version (

## Guards
- Do not touch: - `wit/**` (copies, SH-24's), `templates/**`, `themes/**`, `plugins/markdown|charts|calendar|resolution` (WK-14…17). - `index.json` beyond the three entries (SH-16's validator must stay green).
- Gate: the project's gate
- Accept: Three plugins build to `wasm32-wasip2` and validate; each manifest declares exactly the capabilities it uses (`exec:<program>` names the bare program; ngrok alone has `net:127.0.0.1`).
- Accept: `tunnel-common`'s tests pin the three scanners on masked real fixtures and the state machine's transitions.
- Accept: cloudflared end to end on the Work desk (US-1): install, grant, start, a public URL in the page and in the QR, an answer through the tunnel, stop, nothing left running.
