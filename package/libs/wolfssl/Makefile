#
# Copyright (C) 2006-2017 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=wolfssl
PKG_VERSION:=3.12.2
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).zip
PKG_SOURCE_URL:=https://www.wolfssl.com/
PKG_HASH:=4993844c4b7919007c4511ec3f987fb06543536c3fc933cb53491bffe9150e49

PKG_FIXUP:=libtool
PKG_INSTALL:=1
PKG_USE_MIPS16:=0
PKG_BUILD_PARALLEL:=1
PKG_LICENSE:=GPL-2.0+
PKG_CPE_ID:=cpe:/a:yassl:cyassl

include $(INCLUDE_DIR)/package.mk

define Package/libwolfssl
  SECTION:=libs
  SUBMENU:=SSL
  CATEGORY:=Libraries
  TITLE:=wolfSSL library
  URL:=http://www.wolfssl.com/
  MAINTAINER:=Alexandru Ardelean <ardeleanalex@gmail.com>
  MENU:=1
  PROVIDES:=libcyassl
endef

define Package/libwolfssl/description
wolfSSL (formerly CyaSSL) is an SSL library optimized for small
footprint, both on disk and for memory use.
endef

define Package/libwolfssl/config
	source "$(SOURCE)/Config.in"
endef

TARGET_CFLAGS += $(FPIC)

# --enable-stunnel needed for OpenSSL API compatibility bits
CONFIGURE_ARGS += \
	--enable-opensslextra \
	--enable-sni \
	--enable-stunnel \
	--disable-examples

ifeq ($(CONFIG_IPV6),y)
CONFIGURE_ARGS += \
        --enable-ipv6
endif

ifeq ($(CONFIG_WOLFSSL_HAS_AES_CCM),y)
CONFIGURE_ARGS += \
	--enable-aesccm
endif

ifneq ($(CONFIG_WOLFSSL_HAS_AES_GCM),y)
CONFIGURE_ARGS += \
	--disable-aesgcm
endif

ifneq ($(CONFIG_WOLFSSL_HAS_CHACHA),y)
CONFIGURE_ARGS += \
	--disable-chacha
endif

ifeq ($(CONFIG_WOLFSSL_HAS_ECC),y)
CONFIGURE_ARGS += \
	--enable-ecc \
	--enable-supportedcurves
endif

ifneq ($(CONFIG_WOLFSSL_HAS_DH),y)
CONFIGURE_ARGS += \
	--enable-dh
endif

ifeq ($(CONFIG_WOLFSSL_HAS_ARC4),n)
CONFIGURE_ARGS += \
	--disable-arc4
endif

ifeq ($(CONFIG_WOLFSSL_HAS_DES3),y)
CONFIGURE_ARGS += \
	--disable-des3
endif

ifeq ($(CONFIG_WOLFSSL_HAS_PSK),y)
CONFIGURE_ARGS += \
	--enable-psk
endif

ifeq ($(CONFIG_WOLFSSL_HAS_DTLS),y)
CONFIGURE_ARGS += \
	--enable-dtls
endif

ifeq ($(CONFIG_WOLFSSL_HAS_ECC25519),y)
CONFIGURE_ARGS += \
	--enable-curve25519
endif

ifneq ($(CONFIG_WOLFSSL_HAS_POLY1305),y)
CONFIGURE_ARGS += \
	--enable-poly1305
endif

#ifneq ($(CONFIG_TARGET_x86),)
#	CONFIGURE_ARGS += --enable-intelasm
#endif
#ifneq ($(CONFIG_TARGET_x86_64),)
#	CONFIGURE_ARGS += --enable-intelasm
#endif

define Build/InstallDev
	$(INSTALL_DIR) $(1)/opt/include $(1)/opt/lib/pkgconfig
	$(CP) $(PKG_INSTALL_DIR)/opt/include/* $(1)/opt/include/

	$(INSTALL_DIR) $(1)/opt/lib
	$(CP) $(PKG_INSTALL_DIR)/opt/lib/libwolfssl.{so*,la} $(1)/opt/lib/
	ln -s libwolfssl.so $(1)/opt/lib/libcyassl.so
	ln -s libwolfssl.la $(1)/opt/lib/libcyassl.la

	$(CP) $(PKG_INSTALL_DIR)/opt/lib/pkgconfig/*.pc $(1)/opt/lib/pkgconfig
endef

define Package/libwolfssl/install
	$(INSTALL_DIR) $(1)/opt/lib
	$(CP) $(PKG_INSTALL_DIR)/opt/lib/libwolfssl.so* $(1)/opt/lib/
	ln -s libwolfssl.so $(1)/opt/lib/libcyassl.so
endef

$(eval $(call BuildPackage,libwolfssl))
