# SH-25 — The tunnel plugin: cloudflared, ngrok, Tailscale

Wave: 1 · Run: single · Depends on: pito-work/WK-20
Touches: Cargo.toml, Cargo.lock, plugin.toml, src/, tests/, README.md

## What
A tunnel plugin offers three provider adapters, explicit process permissions, status and a public URL through the remote world.

## How
1. Create shared tunnel state and masked output parsing in a new `src/` module.
2. Add cloudflared, tailscale and ngrok adapters with explicit executable permissions in the component manifest.
3. Build and validate the component; check process lifecycle and malformed output using stand-ins without starting a real tunnel.

## Guards
- Do not touch: `wit/`, `templates/`, `themes/`, the other plugins.
- Gate: the project's gate
- Accept: The component validates and every provider reports failures without disclosing credentials.
