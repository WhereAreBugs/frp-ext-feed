# OpenWrt FRP Ext Feed

本 feed 包含基于下游 FRP 分支构建的包：

- `frpc-ext`：来自 https://github.com/WhereAreBugs/frp.git 的客户端，安装为 `/usr/bin/frpc-ext`。
- `frps-ext`：同一分支的服务端，安装为 `/usr/bin/frps-ext`。
- `luci-app-frpc-ext`：针对 `frpc-ext` 的 LuCI 界面，源码来自 https://github.com/WhereAreBugs/luci-app-frpc-ext.git。
- `luci-app-frps-ext`：针对 `frps-ext` 的 LuCI 界面，源码来自 https://github.com/WhereAreBugs/luci-app-frps-ext.git。

## 与上游的主要差异
- 多路 TCPMux：支持多底层 TCP 会话，按探测/被动信号评分自动择优；支持主动/被动探测，自动检测服务端是否支持新流 Ping，否则回退被动。
- 会话切换对业务 payload 透明，并考虑活跃流量的负载惩罚。
- 多 FRPS 并行连接与负载均衡：`servers[]` 配置多服务端，按每个 proxy 的 `serverNames` 允许列表注册，支持每个服务端独立 token。
- 可执行文件使用 `-ext` 后缀，避免与上游 `frpc`/`frps` 冲突。

## 使用方法
1) 在 OpenWrt `feeds.conf` 添加：
   ```
   src-git frp-ext https://github.com/WhereAreBugs/frp-ext-feed.git
   ```
   （本地调试可用 `src-link frp-ext /path/to/frp-ext-feed`）
2) 更新并安装：
   ```
   ./scripts/feeds update frp-ext
   ./scripts/feeds install frpc-ext frps-ext luci-app-frpc-ext luci-app-frps-ext
   ```
3) 在 `make menuconfig` 选择：
   - Network → frpc-ext / frps-ext
   - LuCI → Applications → luci-app-frpc-ext / luci-app-frps-ext

## 构建说明
- 使用 `golang-package.mk`，源码取自 `https://github.com/WhereAreBugs/frp.git`。
- LuCI 源码取自：
  - `https://github.com/WhereAreBugs/luci-app-frpc-ext.git`
  - `https://github.com/WhereAreBugs/luci-app-frps-ext.git`
- Git 源默认 `PKG_MIRROR_HASH:=skip`，如需固定版本可改为打包 tarball 并填写哈希。
