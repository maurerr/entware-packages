#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
# Entware: no uclibc++

include $(TOPDIR)/rules.mk

PKG_NAME:=zerotier
PKG_VERSION:=1.4.6
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://codeload.github.com/zerotier/ZeroTierOne/tar.gz/$(PKG_VERSION)?
PKG_HASH:=d1a0eeb03acfa446f67adf5901902d17de14b4648c21e160024acf476e3d4fba
PKG_BUILD_DIR:=$(BUILD_DIR)/ZeroTierOne-$(PKG_VERSION)

PKG_MAINTAINER:=Moritz Warning <moritzwarning@web.de>
PKG_LICENSE:=BSL 1.1
PKG_LICENSE_FILES:=LICENSE.txt

PKG_BUILD_PARALLEL:=1
include $(INCLUDE_DIR)/package.mk

define Package/zerotier
  SECTION:=net
  CATEGORY:=Network
  DEPENDS:=+libpthread +libstdcpp +ZEROTIER_ENABLE_NETWORK_CONTROLLER:libsqlite3 +ZEROTIER_USE_MINIUPNPC:libminiupnpc +ZEROTIER_USE_MINIUPNPC:libnatpmp
  TITLE:=Create flat virtual Ethernet networks of almost unlimited size
  URL:=https://www.zerotier.com
  SUBMENU:=VPN
endef

define Package/zerotier/description
	ZeroTier creates a global provider-independent virtual private cloud network.
endef

define Package/zerotier/config
	source "$(SOURCE)/Config.in"
endef

ifeq ($(CONFIG_ZEROTIER_ENABLE_NETWORK_CONTROLLER),y)
MAKE_FLAGS += ZT_ENABLE_NETWORK_CONTROLLER=1
MAKE_FLAGS += LDLIBS+=" -lsqlite3 "
endif

ifeq ($(CONFIG_ZEROTIER_USE_MINIUPNPC),y)
MAKE_FLAGS += ZT_USE_MINIUPNPC=1
endif

ifeq ($(CONFIG_ZEROTIER_ENABLE_DEBUG),y)
TARGET_CXXFLAGS += -DZT_TRACE -ggdb3
endif

#MAKE_FLAGS += \
#	DEFS="" \
#	LDFLAGS+=" -pthread " \
#	LDLIBS+=" -fno-builtin -nodefaultlibs -Wl,-Bstatic -Wl,-Bdynamic -lpthread -lm -lc -lsupc++ -lc -lgcc -lgcc_eh -lgcc_s -lssp_nonshared " \
#	CXXFLAGS+=" -fno-builtin -nostdinc++ -DGCC_HASCLASSVISIBILITY -Wall -fPIE -fvisibility=hidden "

define Build/Compile
	$(call Build/Compile/Default,one)
ifeq ($(CONFIG_ZEROTIER_ENABLE_SELFTEST),y)
	$(call Build/Compile/Default,selftest)
endif
endef

# Make binary smaller
TARGET_CFLAGS += -ffunction-sections -fdata-sections
TARGET_LDFLAGS += -Wl,--gc-sections,--as-needed

ifdef CONFIG_USE_UCLIBC
	TARGET_CFLAGS += -D'valloc(a)=aligned_alloc(getpagesize(),a)'
endif

#define Package/zerotier/conffiles
#/etc/config/zerotier
#endef

define Package/zerotier/install
	$(INSTALL_DIR) $(1)/opt/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/zerotier-one $(1)/opt/bin/
	$(LN) zerotier-one $(1)/opt/bin/zerotier-cli
	$(LN) zerotier-one $(1)/opt/bin//zerotier-idtool

ifeq ($(CONFIG_ZEROTIER_ENABLE_SELFTEST),y)
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/zerotier-selftest $(1)/opt/bin/
endif
endef

$(eval $(call BuildPackage,zerotier))

