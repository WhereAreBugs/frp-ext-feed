include $(TOPDIR)/rules.mk

PKG_NAME:=luci-app-frps-ext
PKG_VERSION:=0.66.2
PKG_RELEASE:=6
PKG_LICENSE:=MIT
PKG_MAINTAINER:=WhereAreBugs

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/WhereAreBugs/luci-app-frps-ext.git
PKG_SOURCE_VERSION:=0.66.2-ext3
PKG_HASH:=skip
PKG_MIRROR_HASH:=skip
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)

LUCI_TITLE:=LuCI support for frps-ext
LUCI_PKGARCH:=all
LUCI_DEPENDS:=+frps-ext

define Package/$(PKG_NAME)/conffiles
/etc/config/frps_ext
endef

define Package/$(PKG_NAME)/postinst
#!/bin/sh
if [ -z "$${IPKG_INSTROOT}" ]; then
	if [ -f "/etc/uci-defaults/40_luci-frps_ext" ]; then
		. /etc/uci-defaults/40_luci-frps_ext
		rm -f /etc/uci-defaults/40_luci-frps_ext
	fi
	rm -f /tmp/luci-indexcache /tmp/luci-modulecache
fi
exit 0
endef

include $(TOPDIR)/feeds/luci/luci.mk

define Build/Prepare
	$(call Build/Prepare/Default)
endef

define Build/Configure
endef

define Build/Compile
endef

$(eval $(call BuildPackage,$(PKG_NAME)))
