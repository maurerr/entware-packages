
include $(TOPDIR)/rules.mk

PKG_NAME:=tmux
PKG_VERSION:=2.9a
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://github.com/tmux/tmux/releases/download/$(PKG_VERSION)
PKG_HASH:=839d167a4517a6bffa6b6074e89a9a8630547b2dea2086f1fad15af12ab23b25
PKG_MAINTAINER:=Maxim Storchak <m.storchak@gmail.com>

PKG_LICENSE:=ISC
PKG_LICENSE_FILES:=COPYING
PKG_CPE_ID:=cpe:/a:nicholas_marriott:tmux

PKG_BUILD_PARALLEL:=1
PKG_INSTALL:=1
PKG_FIXUP:=autoreconf

include $(INCLUDE_DIR)/package.mk

define Package/tmux
  SECTION:=utils
  CATEGORY:=Utilities
  SUBMENU:=Terminal
  TITLE:=Terminal multiplexer
  DEPENDS:=+libncurses +libevent2 +libpthread +librt
  URL:=http://tmux.github.io/
endef

define Package/tmux/description
  tmux is a modern, BSD-licensed alternative to GNU screen.
endef

define Package/tmux/conffiles
/opt/etc/tmux.conf
endef

define Package/tmux/install
	$(INSTALL_DIR) $(1)/opt/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/opt/bin/tmux $(1)/opt/bin/
endef

$(eval $(call BuildPackage,tmux))
