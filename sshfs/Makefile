# 
# Copyright (C) 2013 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=sshfs
PKG_VERSION:=2.4
PKG_RELEASE:=2

PKG_SOURCE:=$(PKG_NAME)-fuse-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=@SF/fuse
PKG_MD5SUM:=3c7c3647c52ce84d09486f1da3a3ce24

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-fuse-$(PKG_VERSION)

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/nls.mk

define Package/sshfs
  TITLE:=SSHFS
  DEPENDS:=+libfuse +fuse-utils +glib2 +libpthread +openssh-client
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=Filesystem
  URL:=http://fuse.sourceforge.net/
endef

define Package/sshfs/description
	Mount remote system over sftp.
endef

CONFIGURE_VARS += \
	SSHFS_CFLAGS=" \
		-D_FILE_OFFSET_BITS=64 \
		-I$(STAGING_DIR)/usr/include/glib-2.0 \
		-I$(STAGING_DIR)/usr/lib/glib-2.0/include \
		-I$(STAGING_DIR)/usr/include/fuse" \
	SSHFS_LIBS=" \
		-lglib-2.0 -liconv $(if $(INTL_FULL),-lintl) -lfuse -pthread -lgthread-2.0 \
		-L$(STAGING_DIR)/usr/lib"

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR) \
		ARCH="$(LINUX_KARCH)" \
		CROSS_COMPILE="$(TARGET_CROSS)" \
		DESTDIR="$(PKG_INSTALL_DIR)" \
		all install
endef

define Package/sshfs/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/sshfs $(1)/usr/bin/
endef

$(eval $(call BuildPackage,sshfs))
