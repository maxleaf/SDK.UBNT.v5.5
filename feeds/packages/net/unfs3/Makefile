# 
# Copyright (C) 2006 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
# $Id: Makefile 11660 2008-07-05 13:24:04Z florian $

include $(TOPDIR)/rules.mk

PKG_NAME:=unfs3
PKG_VERSION:=0.9.20
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=@SF/$(PKG_NAME)
PKG_MD5SUM:=2920d9cc7c3cb76da0792669bf035650

include $(INCLUDE_DIR)/package.mk

define Package/unfs3
  SECTION:=net
  CATEGORY:=Network
  DEPENDS:=+portmap
  TITLE:=User-space NFSv3 Server
  URL:=http://unfs3.sourceforge.net/
endef

define Package/unfs3/description
	UNFS3 is a user-space implementation of the NFSv3 server
	specification. It provides a daemon for the MOUNT and NFS
	protocols, which are used by NFS clients for accessing files
	on the server.
endef

define Package/unfs3/install
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/$(PKG_NAME).init $(1)/etc/init.d/$(PKG_NAME)
	$(INSTALL_DIR) $(1)/etc
	$(INSTALL_DATA) ./files/$(PKG_NAME).exports $(1)/etc/exports
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/unfsd $(1)/usr/sbin
endef

$(eval $(call BuildPackage,unfs3))
