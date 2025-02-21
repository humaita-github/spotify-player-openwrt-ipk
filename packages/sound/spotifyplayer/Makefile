# SPDX-License-Identifier: GPL-3.0-only
#
# Copyright (C) 2023 Facundo Acevedo

include $(TOPDIR)/rules.mk

PKG_NAME:=spotify-player
PKG_VERSION:=0.20.4
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://codeload.github.com/aome510/spotify-player/tar.gz/v$(PKG_VERSION)?
PKG_HASH:=1d13f47ef4df3415835736f32629d57e331707d781507007ea04217a7dc735d8

PKG_MAINTAINER:=Leonardo
PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=LICENSE

PKG_BUILD_DEPENDS:=rust/host dbus libatomic
PKG_BUILD_PARALLEL:=1
PKG_USE_MIPS16:=0
PKG_BUILD_FLAGS:=no-mips16

RUST_PKG_FEATURES:=alsa-backend

include $(INCLUDE_DIR)/package.mk
include $(TOPDIR)/feeds/packages/lang/rust/rust-package.mk

define Package/spotify-player
  SECTION:=sound
  CATEGORY:=Sound
  TITLE:=Spotify terminal music player
  DEPENDS:= +alsa-lib +libavahi-client +dbus +libatomic
  URL:=https://github.com/aome510/spotify-player
endef

define Package/spotify-player/description
  spotify_player is a fast, easy to use, and configurable terminal music player.
endef

define Package/spotify-player/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/spotify_player $(1)/usr/bin/spotify_player
endef

define Build/Prepare
	# We only use this, since MAKE_PATH doesn't seem to work, so we do it manually
	$(PKG_UNPACK)
	(cd $(PKG_BUILD_DIR) && find . -type f -maxdepth 1 -delete && mv -f ./spotify_player/* ./)
	# We install the librespot dependency manually so that we can apply patches
	(wget https://codeload.github.com/librespot-org/librespot/tar.gz/v0.6.0? -O $(PKG_BUILD_DIR)/librespot.tar.gz)
	(cd $(PKG_BUILD_DIR) && tar -axf librespot.tar.gz)
	# We install the openssl-src dependency manually so that we can apply patches for MIPS64
        #(wget https://codeload.github.com/alexcrichton/openssl-src-rs/tar.gz/main -O $(PKG_BUILD_DIR)/openssl-src-rs.tar.gz)
        #(cd $(PKG_BUILD_DIR) && tar -axf openssl-src-rs.tar.gz)
	$(Build/Patch)
endef

$(eval $(call RustBinPackage,spotify-player))
$(eval $(call BuildPackage,spotify-player,+libatomic))
