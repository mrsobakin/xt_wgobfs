#
# Copyright (C) 2022-2025 Wei Chen <weichen302@gmail.com>
#

include $(TOPDIR)/rules.mk

PKG_NAME:=xtables-wgobfs
PKG_VERSION:=0.6.1
PKG_RELEASE:=1

PKG_BUILD_DIR:=$(BUILD_DIR)/xt_wgobfs-$(PKG_VERSION)
PKG_SOURCE:=xt_wgobfs-$(PKG_VERSION).tar.xz
PKG_SOURCE_URL:= https://github.com/infinet/xt_wgobfs/releases/download/v$(PKG_VERSION)/
PKG_HASH:=e76ab54e7df4869593a2b5d538046cd06d2e336fc7e4f21229adee7bac79cd7b

PKG_LICENSE:=GPL-2.0-only
PKG_LICENSE_FILES:=COPYING

PKG_BUILD_DEPENDS:=iptables
PKG_BUILD_PARALLEL:=1
PKG_INSTALL:=1
PKG_MAINTAINER:=Wei Chen <weichen302@gmail.com>

include $(INCLUDE_DIR)/kernel.mk
include $(INCLUDE_DIR)/package.mk

define Package/xtables-wgobfs/description
	An iptables extension for WireGuard obfuscation
endef

XTLIB_DIR:=/usr/lib/iptables

# uses GNU configure
CONFIGURE_ARGS+= \
	--with-kbuild="$(LINUX_DIR)" \
	--with-xtlibdir="$(XTLIB_DIR)"

MAKE_FLAGS = \
	ARCH="$(LINUX_KARCH)" \
	CROSS_COMPILE="$(TARGET_CROSS)" \
	DESTDIR="$(PKG_INSTALL_DIR)" \
	DEPMOD="/bin/true"

define Build/Install
	mkdir -p $(PKG_INSTALL_DIR)/$(XTLIB_DIR)
	$(call Build/Install/Default)
endef

define Package/iptables-mod-wgobfs
	SECTION:=net
	CATEGORY:=Network
	SUBMENU:=Firewall
	TITLE:=iptables WireGuard obfuscation extension
	URL:=https://github.com/infinet/xt_wgobfs
	DEPENDS:= +iptables +libxtables +kmod-ipt-wgobfs
endef

define Package/iptables-mod-wgobfs/install
	$(INSTALL_DIR) $(1)/$(XTLIB_DIR)
	$(CP) \
		$(PKG_INSTALL_DIR)/$(XTLIB_DIR)/libxt_WGOBFS.so \
		$(1)/$(XTLIB_DIR)
endef

define KernelPackage/ipt-wgobfs
	SUBMENU:=Netfilter Extensions
	TITLE:=WireGuard obfuscation netfilter module
	DEPENDS:=+kmod-ipt-core
	FILES:=$(PKG_BUILD_DIR)/src/xt_WGOBFS.$(LINUX_KMOD_SUFFIX)
	AUTOLOAD:=$(call AutoProbe,xt_WGOBFS)
endef

$(eval $(call BuildPackage,iptables-mod-wgobfs))
$(eval $(call KernelPackage,ipt-wgobfs))
