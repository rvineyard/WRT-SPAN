#
# Copyright (C) 2007-2011 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=daemonlogger
PKG_VERSION:=1.2.1
PKG_RELEASE:=1

PKG_SOURCE:=463
PKG_SOURCE_URL:=http://www.snort.org/downloads/
PKG_CAT:=zcat
PKG_MD5SUM:=acb64aa6cd5777e297569f100b5c39ee

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)/$(PKG_NAME)-$(PKG_VERSION)

PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/daemonlogger
  SECTION:=net
  CATEGORY:=Network
  DEPENDS:=+libpcap
  DEPENDS:=+libdnet
  TITLE:=Software Network Tap
  URL:=http://www.snort.org/snort-downloads/additional-downloads
endef

CONFIGURE_VARS += \
	BUILD_CC="$(TARGET_CC)" \
	HOSTCC="$(HOSTCC)"

MAKE_FLAGS := CCOPT="$(TARGET_CFLAGS)" INCLS="-I. $(TARGET_CPPFLAGS)"

define Build/Configure
	$(call Build/Configure/Default)
endef

define Package/daemonlogger/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/daemonlogger $(1)/usr/bin/
endef

$(eval $(call BuildPackage,daemonlogger))
