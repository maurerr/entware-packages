#
# Copyright (C) 2014 - 2018 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=libxslt
PKG_VERSION:=1.1.33
PKG_RELEASE:=3

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:= \
	http://xmlsoft.org/sources/ \
	ftp://fr.rpmfind.net/pub/libxml/
PKG_HASH:=8e36605144409df979cab43d835002f63988f3dc94d5d3537c12796db90e38c8

PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=COPYING
PKG_CPE_ID:=cpe:/a:xmlsoft:libxslt

PKG_MAINTAINER:=Jiri Slachta <jiri@slachta.eu>

PKG_FIXUP:=autoreconf
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/libxslt
  SECTION:=libs
  CATEGORY:=Libraries
  DEPENDS:=+libxml2
  TITLE:=Gnome XSLT library
  URL:=http://xmlsoft.org/XSLT/
endef

define Package/libxslt/description
 A library for XML transformation using XSLT.
endef

define Package/libexslt
  SECTION:=libs
  CATEGORY:=Libraries
  DEPENDS:=+libxslt
  TITLE:=Gnome XSLT library Extension
  URL:=http://xmlsoft.org/XSLT/EXSLT/
endef

define Package/libexslt/description
 An extension for XSLT.
endef

define Package/xsltproc
  SECTION:=utils
  CATEGORY:=Utilities
  DEPENDS:=+libxslt +PACKAGE_xsltproc:libexslt
  TITLE:=Gnome XSLT xsltproc Utility
  URL:=http://xmlsoft.org/XSLT/
endef

define Package/xsltproc/description
 XSLT XML transformation utility.
endef

CONFIGURE_ARGS+= \
	--disable-silent-rules \
	--enable-shared \
	--enable-static \
	--without-python \
	--without-crypto \
	--without-debug \
	--without-mem-debug \
	--without-debugger \

define Build/InstallDev/Xslt
	$(INSTALL_DIR) $(1)/opt/bin $(2)/bin $(1)/opt/include/libxslt \
		$(1)/opt/include/libexslt $(1)/opt/lib \
		$(1)/opt/lib/pkgconfig $(2)/share/aclocal

	$(INSTALL_BIN) \
		$(PKG_INSTALL_DIR)/opt/bin/xslt-config \
		$(2)/bin/

	ln -sf $(STAGING_DIR)/host/bin/xslt-config $(1)/opt/bin/xslt-config

	$(SED) \
		's,^\(prefix\|exec_prefix\)=.*,\1=$(STAGING_DIR)/opt,g' \
		$(2)/bin/xslt-config

	$(INSTALL_DATA) \
		$(PKG_INSTALL_DIR)/opt/include/libxslt/* \
		$(1)/opt/include/libxslt/

	$(CP) \
		$(PKG_INSTALL_DIR)/opt/lib/libxslt.{la,a,so*} \
		$(1)/opt/lib/

	$(INSTALL_DATA) \
		$(PKG_INSTALL_DIR)/opt/lib/pkgconfig/libxslt.pc \
		$(1)/opt/lib/pkgconfig/

	$(INSTALL_DATA) \
		$(PKG_INSTALL_DIR)/opt/share/aclocal/* \
		$(2)/share/aclocal
endef

define Build/InstallDev/Exslt
	$(INSTALL_DIR) $(1)/opt/include/libexslt $(1)/opt/lib/pkgconfig
	$(INSTALL_DATA) \
		$(PKG_INSTALL_DIR)/opt/include/libexslt/* \
		$(1)/opt/include/libexslt/

	$(CP) \
		$(PKG_INSTALL_DIR)/opt/lib/libexslt.{la,a,so*} \
		$(1)/opt/lib/

	$(INSTALL_DATA) \
		$(PKG_INSTALL_DIR)/opt/lib/pkgconfig/libexslt.pc \
		$(1)/opt/lib/pkgconfig/
endef

define Build/InstallDev
	 $(if $(CONFIG_PACKAGE_libxslt),$(call Build/InstallDev/Xslt,$(1),$(2)))
	 $(if $(CONFIG_PACKAGE_libexslt),$(call Build/InstallDev/Exslt,$(1),$(2)))
endef

define Package/libxslt/install
	$(INSTALL_DIR) $(1)/opt/lib
	$(CP) \
		$(PKG_INSTALL_DIR)/opt/lib/libxslt.so* \
		$(1)/opt/lib/
endef

define Package/libexslt/install
	$(INSTALL_DIR) $(1)/opt/lib
	$(CP) \
		$(PKG_INSTALL_DIR)/opt/lib/libexslt.so* \
		$(1)/opt/lib/
endef

define Package/xsltproc/install
	$(INSTALL_DIR) $(1)/opt/bin
	$(INSTALL_BIN) \
		$(PKG_INSTALL_DIR)/opt/bin/xsltproc \
		$(1)/opt/bin/
endef

$(eval $(call BuildPackage,libxslt))
$(eval $(call BuildPackage,libexslt))
$(eval $(call BuildPackage,xsltproc))
