#
# Copyright (C) 2006-2015 OpenWrt.org
#		2014-2015 Vitaly Protsko
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=gammu
PKG_VERSION:=1.40.0
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.xz
PKG_SOURCE_URL:=https://dl.cihar.com/gammu/releases
PKG_HASH:=a760a3520d9f3a16a4ed73cefaabdbd86125bec73c6fa056ca3f0a4be8478dd6

PKG_MAINTAINER:=Vitaly Protsko <villy@sft.ru>
PKG_LICENCE:=GPL-2.0-or-later
PKG_LICENSE_FILES:=COPYING

PKG_BUILD_PARALLEL:=1
CMAKE_INSTALL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/nls.mk
include $(INCLUDE_DIR)/cmake.mk

define Package/gammu
	SECTION:=utils
	CATEGORY:=Utilities
	TITLE:=Cell phone/modem SMS and control tool
	URL:=https://wammu.eu/gammu/
	DEPENDS:=+libpthread +libcurl +glib2 $(ICONV_DEPENDS) $(INTL_DEPENDS)
	DEPENDS+=+PACKAGE_python:python +PACKAGE_bluez-libs:bluez-libs
	DEPENDS+=+PACKAGE_libmariadb:libmariadb +PACKAGE_unixodbc:unixodbc +PACKAGE_libpq:libpq
	DEPENDS+=+PACKAGE_libusb-1.0:libusb-1.0
endef

CMAKE_OPTIONS += -DWITH_LibDBI:BOOL=OFF
ifneq ($(INTL_FULL),)
CMAKE_OPTIONS += -DLIBINTL_INCLUDE_DIRS:PATH=$(INTL_PREFIX)/include
CMAKE_OPTIONS += -DLIBINTL_LIBRARIES:STRING=intl
endif
ifneq ($(ICONV_FULL),)
CMAKE_OPTIONS += -DDISABLE_ICONV=OFF
CMAKE_OPTIONS += -DICONV_INCLUDE_DIRS:PATH=$(ICONV_PREFIX)/include
CMAKE_OPTIONS += -DICONV_LIBRARIES:STRING=iconv
endif

define Package/gammu/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/gammu $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/gammu-{smsd,smsd-inject,smsd-monitor} $(1)/usr/bin
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/lib/lib{Gammu*,gsmsd*} $(1)/usr/lib
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_CONF) ./files/gammu $(1)/etc/config/gammu
	$(INSTALL_BIN) ./files/gammu.init $(1)/etc/init.d/gammu
endef

define Package/gammu/conffiles
/etc/config/gammu
endef

$(eval $(call BuildPackage,gammu))
