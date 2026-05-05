include $(TOPDIR)/rules.mk

PKG_NAME:=ntfy
PKG_VERSION:=2.22.0
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://codeload.github.com/binwiederhier/ntfy/tar.gz/v$(PKG_VERSION)?
PKG_HASH:=fe14d88856a4e0d3dec478c224d980f81df707f6ee5b7d68322c6d9ace0e90ab

PKG_LICENSE:=Apache-2.0
PKG_LICENSE_FILES:=LICENSE

PKG_BUILD_DEPENDS:=golang/host node/host
PKG_BUILD_PARALLEL:=1
PKG_BUILD_FLAGS:=no-mips16

GO_PKG:=heckel.io/ntfy/v2
GO_PKG_BUILD_PKG:=$(GO_PKG)
GO_PKG_LDFLAGS_X:= \
	main.version=$(PKG_VERSION) \
	main.commit=$(PKG_VERSION) \
	main.date=$(PKG_VERSION)
GO_PKG_TAGS:=sqlite_omit_load_extension,osusergo,netgo

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
	/etc/ntfy/server.yml
	/etc/ntfy/user.db
endef

define Build/Compile
	mkdir -p $(PKG_BUILD_DIR)/server/docs
	touch $(PKG_BUILD_DIR)/server/docs/index.html
	cd $(PKG_BUILD_DIR)/web; \
	npm ci; \
	npm run build; \
	mv build/index.html build/app.html; \
	mv build ../server/site; \
	rm ../server/site/config.js
	$(call GoPackage/Build/Compile)
endef

define Package/ntfy/install
	$(call GoPackage/Package/Install/Bin,$(1))

	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/ntfy.init $(1)/etc/init.d/ntfy

	$(INSTALL_DIR) $(1)/etc/ntfy
	$(INSTALL_CONF) $(PKG_BUILD_DIR)/server/server.yml $(1)/etc/ntfy/server.yml
endef

$(eval $(call GoBinPackage,ntfy))
$(eval $(call BuildPackage,ntfy))
