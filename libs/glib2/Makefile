#
# Copyright (C) 2007-2019 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=glib2
PKG_VERSION:=2.58.3
PKG_RELEASE:=3

PKG_SOURCE:=glib-$(PKG_VERSION).tar.xz
PKG_BUILD_DIR:=$(BUILD_DIR)/glib-$(PKG_VERSION)
PKG_SOURCE_URL:=@GNOME/glib/2.58
PKG_HASH:=8f43c31767e88a25da72b52a40f3301fefc49a665b56dc10ee7cc9565cbe7481

PKG_BUILD_PARALLEL:=1
HOST_BUILD_PARALLEL:=1
PKG_BUILD_DEPENDS:=gettext
HOST_BUILD_DEPENDS:=gettext-full/host libiconv/host libffi/host
PKG_INSTALL:=1
PKG_USE_MIPS16:=0

PKG_CPE_ID:=cpe:/a:gnome:glib

PKG_FIXUP:=autoreconf

HOST_BUILD_DIR:=$(BUILD_DIR_HOST)/glib-$(PKG_VERSION)

include $(INCLUDE_DIR)/host-build.mk
include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/nls.mk

define Package/glib2
  SECTION:=libs
  CATEGORY:=Libraries
  DEPENDS:=$(ICONV_DEPENDS) $(INTL_DEPENDS) +zlib +libpthread +libffi +libattr
  TITLE:=glib 2.0
  MAINTAINER:=Peter Wagner <tripolar@gmx.at>
  URL:=http://www.gtk.org/
endef

define Package/glib2/description
  The GLib library of C routines
endef

TARGET_CFLAGS += -Wno-error=implicit-function-declaration

HOST_CONFIGURE_ARGS += \
	--disable-libelf \
	--disable-selinux \
	--with-libiconv=gnu \
	--with-pcre=internal \
	--disable-libmount

CONFIGURE_ARGS += \
	--enable-shared \
	--enable-static \
	--disable-debug \
	--disable-selinux \
	--disable-libmount \
	--disable-fam \
	--disable-gtk-doc-html \
	--disable-man \
	--with-libiconv=gnu \
	--with-pcre=internal

CONFIGURE_VARS += \
	glib_cv_stack_grows=no \
	glib_cv_uscore=no \
	ac_cv_path_GLIB_GENMARSHAL=$(STAGING_DIR_HOSTPKG)/bin/glib-genmarshal \
	ac_cv_func_mmap_fixed_mapped=yes \
	ac_cv_func_posix_getpwuid_r=yes \
	ac_cv_func_posix_getgrgid_r=yes

ifeq ($(findstring $(CONFIG_GCC_USE_VERSION),7),)
TARGET_CFLAGS += -Wno-error=missing-include-dirs
endif

define Build/InstallDev
	$(FIND) $(PKG_INSTALL_DIR) -name '*.pc' | $(XARGS) $(SED) \
		's,^bindir=.*,bindir=$(STAGING_DIR)/opt/bin,'

	$(INSTALL_DIR) $(1)/opt/bin
	$(CP) \
		$(PKG_INSTALL_DIR)/opt/bin/* \
		$(1)/opt/bin/

	$(INSTALL_DIR) $(1)/opt/include
	$(CP) \
		$(PKG_INSTALL_DIR)/opt/include/glib-2.0 \
		$(1)/opt/include/
	$(CP) \
		$(PKG_INSTALL_DIR)/opt/lib/glib-2.0/include/*.h \
		$(1)/opt/include/glib-2.0/
	$(CP) \
		$(PKG_INSTALL_DIR)/opt/include/gio-unix-2.0 \
		$(1)/opt/include/

	$(INSTALL_DIR) $(1)/opt/lib
	$(CP) \
		$(PKG_INSTALL_DIR)/opt/lib/glib-2.0 \
		$(1)/opt/lib/

	$(CP) \
		$(PKG_INSTALL_DIR)/opt/lib/*.{so*,a,la} \
		$(1)/opt/lib/

	$(INSTALL_DIR) $(1)/opt/lib/pkgconfig
	$(INSTALL_DATA) \
		$(PKG_INSTALL_DIR)/opt/lib/pkgconfig/*.pc \
		$(1)/opt/lib/pkgconfig

	$(INSTALL_DIR) $(2)/share/aclocal/
	$(INSTALL_DATA) \
		$(PKG_INSTALL_DIR)/opt/share/aclocal/*.m4  \
		$(2)/share/aclocal/
endef

define Package/glib2/install
	$(INSTALL_DIR) $(1)/opt/lib
	$(CP) \
		$(PKG_INSTALL_DIR)/opt/lib/*.so* \
		$(1)/opt/lib/
endef

$(eval $(call HostBuild))
$(eval $(call BuildPackage,glib2))
