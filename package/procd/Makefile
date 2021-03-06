include $(TOPDIR)/rules.mk

PKG_NAME:=procd
PKG_VERSION:=2013-06-10
PKG_RELEASE=$(PKG_SOURCE_VERSION)

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=git://nbd.name/luci2/procd.git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=c261d39c1ae2db872495c2129c05d1154f46b5c3
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.gz
CMAKE_INSTALL:=1

PKG_LICENSE:=GPLv2
PKG_LICENSE_FILES:=

PKG_CONFIG_DEPENDS:=CONFIG_PROCD_INIT

PKG_MAINTAINER:=John Crispin <blogic@openwrt.org>

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

TARGET_LDFLAGS += $(if $(CONFIG_USE_EGLIBC),-lrt)

define Package/procd
  SECTION:=base
  CATEGORY:=Base system
  DEPENDS:=+ubusd +ubus +libjson-script +USE_EGLIBC:librt +PROCD_INIT:ubox
  TITLE:=OpenWrt system process manager
endef

ifneq ($(CONFIG_PROCD_INIT),)
define Package/procd/install
	$(INSTALL_DIR) $(1)/sbin $(1)/lib/functions $(1)/etc

	$(CP) $(PKG_INSTALL_DIR)/usr/sbin/{procd,askfirst,udevtrigger,logread} $(1)/sbin/
	$(INSTALL_DATA) ./files/hotplug*.json $(1)/etc/
	$(INSTALL_DATA) ./files/procd.sh $(1)/lib/functions/
	ln -s /sbin/procd $(1)/sbin/init
endef
else
define Package/procd/install
	$(INSTALL_DIR) $(1)/sbin $(1)/lib/functions $(1)/etc/init.d

	$(CP) $(PKG_INSTALL_DIR)/usr/sbin/procd $(1)/sbin/
	$(INSTALL_BIN) ./files/procd.init $(1)/etc/init.d/procd
	$(INSTALL_DATA) ./files/procd.sh $(1)/lib/functions/
endef
endif

$(eval $(call BuildPackage,procd))
