#
# Copyright (C) 2011-2015 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=rng-tools
PKG_VERSION:=6.7
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/nhorman/rng-tools
PKG_SOURCE_VERSION:=v$(PKG_VERSION)
PKG_MIRROR_HASH:=05cb68b8600025f362ea0875f5966b60f8195f91ed89b431996a48cd88b1e5b0

PKG_MAINTAINER:=Nathaniel Wesley Filardo <nwfilardo@gmail.com>
PKG_LICENSE:=GPL-2.0-or-later
PKG_LICENSE_FILES:=COPYING

PKG_FIXUP:=autoreconf
PKG_INSTALL:=1
PKG_BUILD_PARALLEL:=1
PKG_BUILD_DEPENDS:=!USE_GLIBC:argp-standalone

include $(INCLUDE_DIR)/package.mk

define Package/rng-tools
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=Daemon for adding entropy to kernel entropy pool
  URL:=https://github.com/nhorman/rng-tools
  DEPENDS:=+libsysfs +libpthread
endef

define Package/rng-tools/description
  Daemon for adding entropy to kernel entropy pool. By default it uses
  /dev/urandom as the source but the init script can be modified
  to use a hardware source like /dev/hwrng if present
endef

CONFIGURE_ARGS += \
	--without-libgcrypt \
	--without-nistbeacon \
	--without-pkcs11

ifndef CONFIG_USE_GLIBC
	CONFIGURE_VARS += LIBS="-largp"
endif

define Build/Prepare
	$(call Build/Prepare/Default)
	(cd $(PKG_BUILD_DIR); ln -s README.md README)
endef

define Package/rng-tools/install
	$(INSTALL_DIR) $(1)/opt/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/opt/bin/rngtest $(1)/opt/bin/
	$(INSTALL_DIR) $(1)/opt/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/opt/sbin/rngd $(1)/opt/sbin/
endef

$(eval $(call BuildPackage,rng-tools))
