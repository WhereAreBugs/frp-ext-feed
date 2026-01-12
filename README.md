# FRP Ext Feed for OpenWrt

This feed bundles three packages built from the downstream FRP fork:

- `frpc-ext`: FRP client built from https://github.com/WhereAreBugs/frp.git, installed as `/usr/bin/frpc-ext`.
- `frps-ext`: FRP server built from the same fork, installed as `/usr/bin/frps-ext`.
- `luci-app-frpc-ext`: LuCI UI for `frpc-ext`, sourced from https://github.com/WhereAreBugs/luci-app-frpc-ext.git.

## Notable changes vs upstream
- Multi-session TCPMux with per-session quality scoring and active/passive link probing (probe support auto-detection; fallback to passive when server doesn’t support Ping-on-new-stream).
- Automatic session selection with active-stream penalties; switching is payload-transparent.
- Multi-FRPS concurrent connection & load balancing (`servers[]` + per-proxy `serverNames` allow list; per-server token override).
- Binary names are suffixed with `-ext` to avoid clashing with upstream `frpc`/`frps`.
- LuCI defaults are aligned to `/usr/bin/frpc-ext` and depend on the `frpc-ext` package.

## How to use
1) Add this feed to your OpenWrt `feeds.conf`:
   ```
   src-git frp-ext https://github.com/WhereAreBugs/frp-ext-feed.git
   ```
   (for local testing: `src-link frp-ext /path/to/frp-ext-feed`)
2) Update and install:
   ```
   ./scripts/feeds update frp-ext
   ./scripts/feeds install frpc-ext frps-ext luci-app-frpc-ext
   ```
3) Select packages in `make menuconfig`:
   - Network → frpc-ext / frps-ext
   - LuCI → Applications → luci-app-frpc-ext

## Build notes
- Go toolchain: uses `golang-package.mk` and builds from the fork at `https://github.com/WhereAreBugs/frp.git`.
- LuCI package is fetched from `https://github.com/WhereAreBugs/luci-app-frpc-ext.git`.
- `PKG_MIRROR_HASH` is set to `skip` for git sources; adjust if you vendor tarballs.
