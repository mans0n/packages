include $(TOPDIR)/rules.mk

PKG_NAME:=7-zip
PKG_VERSION:=23.01
PKG_SOURCE_NAME:=7z
PKG_SOURCE_VERSION:=2301

PKG_SOURCE:=$(PKG_SOURCE_NAME)$(PKG_SOURCE_VERSION)-src.tar.xz
PKG_SOURCE_URL:=https://7-zip.org/a/
PKG_HASH:=356071007360e5a1824d9904993e8b2480b51b570e8c9faf7c0f58ebe4bf9f74
PKG_CPE_ID:=cpe:/a:7-zip:7zip

# unRAR code is removed from compilation, so this is free from the unRAR license restriction.
PKG_LICENSE:=LGPL-2.1-or-later BSD-3-Clause
PKG_LICENSE_FILES:=DOC/License.txt DOC/copying.txt

PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk

TAR_CMD=$(HOST_TAR) -C $(1) $(TAR_OPTIONS)

ALONE_DIR=CPP/7zip/Bundles/Alone2
MAKE_PATH:=$(ALONE_DIR)
MAKE_FLAGS += \
	-f ../../cmpl_gcc.mak \
	CROSS_COMPILE="$(TARGET_CROSS)" \
	DISABLE_RAR=1

define Package/7-zip
  SECTION:=utils
  CATEGORY:=Utilities
  SUBMENU:=Compression
  DEPENDS:=+libstdcpp
  TITLE:=7-Zip is a compression utility.
  URL:=https://7-zip.org/
endef

define Package/7-zip/description
  7-Zip is a file archiver with a high compression ratio.
endef

define Package/7-zip/install
	$(INSTALL_DIR) $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/$(ALONE_DIR)/b/g/7zz $(1)/usr/bin/
endef

$(eval $(call BuildPackage,7-zip))
