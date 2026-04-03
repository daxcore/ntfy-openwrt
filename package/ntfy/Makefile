include $(TOPDIR)/rules.mk

PKG_NAME:=ntfy
PKG_VERSION:=2.21.0
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://codeload.github.com/binwiederhier/ntfy/tar.gz/v$(PKG_VERSION)?
PKG_HASH:=02323e45a04de175456701c9ff741c8e00602ced22d8fd59842d88ab66e267cf

PKG_LICENSE:=Apache-2.0
PKG_LICENSE_FILES:=LICENSE

PKG_BUILD_DEPENDS:=golang/host
PKG_BUILD_PARALLEL:=1
PKG_BUILD_FLAGS:=no-mips16

#GO_PKG:=github.com/binwiederhier/ntfy
#GO_PKG_BUILD_PKG:=$(GO_PKG)

include $(INCLUDE_DIR)/package.mk
include $(TOPDIR)/feeds/packages/lang/golang/golang-package.mk

define Package/ntfy
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=ntfy - HTTP-based pub-sub notification service
  URL:=https://github.com/binwiederhier/ntfy
  DEPENDS:=$(GO_ARCH_DEPENDS) +libsqlite3
endef

define Package/ntfy/description
  ntfy is a simple HTTP-based pub-sub notification service.
  With ntfy, you can send notifications to your phone or desktop
  via scripts from any computer.
endef

define Package/ntfy/conffiles
/etc/ntfy.yml
endef

define Package/ntfy/install
	$(call GoPackage/Package/Install/Bin,$(1))
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/ntfy $(1)/usr/bin/ntfy

	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/ntfy.init $(1)/etc/init.d/ntfy

	$(INSTALL_DIR) $(1)/etc
	$(INSTALL_CONF) $(PKG_BUILD_DIR)/server/server.yml $(1)/etc/ntfy.yml
endef

$(eval $(call GoBinPackage,ntfy))
$(eval $(call BuildPackage,ntfy))
