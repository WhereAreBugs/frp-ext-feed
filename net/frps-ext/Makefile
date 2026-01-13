include $(TOPDIR)/rules.mk

PKG_NAME:=frps-ext
PKG_VERSION:=0.0.0
PKG_RELEASE:=1
PKG_LICENSE:=Apache-2.0
PKG_MAINTAINER:=WhereAreBugs

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/WhereAreBugs/frp.git
PKG_SOURCE_VERSION:=HEAD
PKG_MIRROR_HASH:=skip
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz

# Keep default PKG_BUILD_DIR layout so go.mod is at source root.
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)

# Default (normal OpenWrt buildroot/SDK): build and use OpenWrt's golang/host.
# CI optimization: set FRP_EXT_USE_SYSTEM_GO=1 to use system Go instead.
ifndef FRP_EXT_USE_SYSTEM_GO
PKG_BUILD_DEPENDS:=golang/host
endif

# Go packages don't work with MIPS16.
# This keeps the package buildable on MIPS targets.
PKG_BUILD_FLAGS:=no-mips16

GO_PKG:=github.com/fatedier/frp
GO_PKG_BUILD_PKG:=./cmd/frps
GO_PKG_TAGS:=frps
GO_PKG_LDFLAGS:=-s -w

include $(INCLUDE_DIR)/package.mk
include $(TOPDIR)/feeds/packages/lang/golang/golang-package.mk

define Package/frps-ext
  SECTION:=net
  CATEGORY:=Network
  TITLE:=FRP server (ext)
  DEPENDS:=+ca-bundle
endef

define Package/frps-ext/description
Downstream FRP server (ext) built from this fork.
endef

define Build/Compile
	$(call GoPackage/Build/Compile)
endef

define Package/frps-ext/install
	$(call GoPackage/Package/Install/Bin,$(1))
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(GO_PKG_BUILD_BIN_DIR)/frps $(1)/usr/bin/frps-ext
endef

$(eval $(call BuildPackage,frps-ext))
