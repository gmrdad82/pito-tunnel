# pito-tunnel

The Tunnel plugin for the PITO desks: reach the Remote module from outside the machine through cloudflared, ngrok or Tailscale, one provider picked on the plugin's page; only the picked program is ever started, and the plugin declares the three it may start.

Under construction: nothing to install yet. When it ships, a tagged
release carries three files, `plugin.toml`, `plugin.wasm` and
`SHA256SUMS`, and any PITO desk with its Remote module on installs it from this repository by name,
verifying the hash before loading anything. Official: the owner of this
repository is the owner of the desks.
