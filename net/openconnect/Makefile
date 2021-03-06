#
# Copyright (C) 2006-2015 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=openconnect
PKG_VERSION:=8.10
PKG_RELEASE:=2

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=ftp://ftp.infradead.org/pub/openconnect/
PKG_HASH:=30e64c6eca4be47bbf1d61f53dc003c6621213738d4ea7a35e5cf1ac2de9bab1

PKG_LICENSE:=LGPL-2.1-or-later
PKG_LICENSE_FILES:=COPYING.LGPL
PKG_CPE_ID:=cpe:/a:infradead:openconnect

PKG_CONFIG_DEPENDS:= \
	CONFIG_OPENCONNECT_GNUTLS \
	CONFIG_OPENCONNECT_OPENSSL \

PKG_USE_MIPS16:=0

include $(INCLUDE_DIR)/package.mk

define Package/openconnect/config
	source "$(SOURCE)/Config.in"
endef

define Package/openconnect
  SECTION:=net
  CATEGORY:=Network
  DEPENDS:=+libxml2 +resolveip +vpnc-scripts +OPENCONNECT_OPENSSL:libopenssl +OPENCONNECT_OPENSSL:p11-kit +OPENCONNECT_OPENSSL:libp11 +OPENCONNECT_GNUTLS:libgnutls +OPENCONNECT_GNUTLS:libtasn1 +OPENCONNECT_STOKEN:libstoken
  TITLE:=OpenConnect VPN client (Cisco AnyConnect and Juniper/Pulse compatible)
  MAINTAINER:=Nikos Mavrogiannopoulos <n.mavrogiannopoulos@gmail.com>
  URL:=https://www.infradead.org/openconnect/
  SUBMENU:=VPN
endef

define Package/openconnect/description
	A VPN client compatible with Cisco's AnyConnect SSL VPN, ocserv and Juniper (Pulse secure).

        OpenConnect is a client that follows the Cisco's AnyConnect SSL VPN protocol,
        which is supported by IOS 12.4(9)T or later on Cisco SR500, 870, 880, 1800,
        2800, 3800, 7200 Series and Cisco 7301 Routers, as well as the OpenConnect
        VPN server. It has later been ported to support the Juniper SSL VPN which
        is now known as Pulse Connect Secure.
endef

CONFIGURE_ARGS += \
	--disable-shared \
	--with-vpnc-script=/opt/lib/netifd/vpnc-script \
	--without-libpcsclite \
	--without-stoken \
	--without-libpskc \
	--without-gssapi \
	--without-lz4

ifeq ($(CONFIG_OPENCONNECT_OPENSSL),y)
CONFIGURE_ARGS += \
	--without-gnutls
endif

ifeq ($(CONFIG_OPENCONNECT_STOKEN),y)
CONFIGURE_ARGS += \
	--with-stoken
endif

define Package/openconnect/install
	$(INSTALL_DIR) $(1)/opt/etc/openconnect/
	$(INSTALL_DIR) $(1)/opt/lib/netifd/proto
	$(INSTALL_BIN) ./files/openconnect.sh $(1)/opt/lib/netifd/proto/
	$(INSTALL_DIR) $(1)/opt/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/openconnect $(1)/opt/sbin/
	$(INSTALL_BIN) ./files/openconnect-wrapper $(1)/opt/sbin/
	$(INSTALL_DIR) $(1)/opt/lib/upgrade/keep.d
	$(INSTALL_DATA) ./files/openconnect.upgrade $(1)/opt/lib/upgrade/keep.d/openconnect
endef

$(eval $(call BuildPackage,openconnect))
