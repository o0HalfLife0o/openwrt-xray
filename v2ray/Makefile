# SPDX-License-Identifier: GPL-3.0-only
#
# Copyright (C) 2021 ImmortalWrt.org

include $(TOPDIR)/rules.mk

PKG_NAME:=v2ray-core
PKG_VERSION:=5.21.0
PKG_RELEASE:=$(AUTORELEASE)

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://codeload.github.com/v2fly/v2ray-core/tar.gz/v$(PKG_VERSION)?
PKG_HASH:=880a929caff7b72ef9d3b9a3262cec0dff6566c2481989822a6b27fdaaeed975

PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=LICENSE
PKG_MAINTAINER:=Tianling Shen <cnsztl@immortalwrt.org>

PKG_BUILD_DEPENDS:=golang/host
PKG_BUILD_PARALLEL:=1
PKG_USE_MIPS16:=0

GO_PKG:=github.com/v2fly/v2ray-core/v5
GO_PKG_BUILD_PKG:=$(GO_PKG)/main
GO_PKG_LDFLAGS_X:= \
	$(GO_PKG).build=OpenWrt \
	$(GO_PKG).version=$(PKG_VERSION)

include $(INCLUDE_DIR)/package.mk
include $(TOPDIR)/feeds/packages/lang/golang/golang-package.mk

define Package/v2ray/template
  TITLE:=A platform for building proxies to bypass network restrictions
  SECTION:=net
  CATEGORY:=Network
  URL:=https://www.v2fly.org
endef

define Package/v2ray-core
  $(call Package/v2ray/template)
  DEPENDS:=$(GO_ARCH_DEPENDS) +ca-bundle
endef

define Package/v2ray/description
  Project V is a set of network tools that help you to build your own computer network.
  It secures your network connections and thus protects your privacy.
endef

define Package/v2ray-core/description
  $(call Package/v2ray/description)
endef

define Package/v2ray-core/conffiles
/etc/config/v2ray
/etc/v2ray/
endef

define Package/v2ray-core/install
	$(call GoPackage/Package/Install/Bin,$(PKG_INSTALL_DIR))
	$(INSTALL_DIR) $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/main $(1)/usr/bin/v2ray

	$(INSTALL_DIR) $(1)/etc/v2ray/
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/release/config/config.json $(1)/etc/v2ray/

	$(INSTALL_DIR) $(1)/etc/config/
	$(INSTALL_CONF) $(CURDIR)/files/v2ray.conf $(1)/etc/config/v2ray
	$(INSTALL_DIR) $(1)/etc/init.d/
	$(INSTALL_BIN) $(CURDIR)/files/v2ray.init $(1)/etc/init.d/v2ray
endef

$(eval $(call BuildPackage,v2ray-core))