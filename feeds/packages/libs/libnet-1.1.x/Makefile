# 
# Copyright (C) 2006 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
# $Id: Makefile 12228 2008-08-06 22:43:15Z nbd $

include $(TOPDIR)/rules.mk

PKG_NAME:=libnet
PKG_VERSION:=1.1.2.1
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME).tar.gz
PKG_SOURCE_URL:=http://www.packetfactory.net/libnet/dist/
PKG_MD5SUM:=be845c41170d72c7db524f3411b50256

PKG_BUILD_DIR:=$(BUILD_DIR)/libnet

include $(INCLUDE_DIR)/package.mk

define Package/libnet1
  SECTION:=libs
  CATEGORY:=Libraries
  DEPENDS:=+libpcap
  TITLE:=Low-level packet creation library (v1.1.x)
  URL:=http://www.packetfactory.net/libnet/
endef

ENDIANESS:=lil
ifeq ($(CONFIG_BIG_ENDIAN),y)
  ENDIANESS:=big
endif

TARGET_CFLAGS += $(FPIC)

CONFIGURE_ARGS += \
	--enable-shared \
	--enable-static \
	--with-pf_packet=yes

CONFIGURE_VARS += \
	ac_cv_libnet_endianess=$(ENDIANESS) \
	ac_libnet_have_pf_packet=yes \
	LL_INT_TYPE=linux

define Build/Configure
	(cd $(PKG_BUILD_DIR); touch \
		configure.in \
		include.m4 \
		aclocal.m4 \
		Makefile.in \
		configure \
	);
	$(call Build/Configure/Default)
endef

define Build/Compile
	# pass CFLAGS again to override -O2 set by configure
	$(MAKE) -C $(PKG_BUILD_DIR) \
		CFLAGS="$(TARGET_CFLAGS)" \
		DESTDIR="$(PKG_INSTALL_DIR)" \
		all install
endef

define Build/InstallDev
	mkdir -p $(1)/usr/lib/libnet-1.1.x/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/libnet-config $(1)/usr/lib/libnet-1.1.x/bin/
	mkdir -p $(1)/usr/lib/libnet-1.1.x/include
	$(CP) $(PKG_INSTALL_DIR)/usr/include/libnet* $(1)/usr/lib/libnet-1.1.x/include/
	mkdir -p $(1)/usr/lib/libnet-1.1.x/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libnet.{a,so*} $(1)/usr/lib/libnet-1.1.x/lib/
endef

define Package/libnet1/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libnet.so.* $(1)/usr/lib/
endef

$(eval $(call BuildPackage,libnet1))
