#
# Copyright (C) 2011 Johannes Weißl <jargon@molb.org>
# Copyright (C) 2006-2011 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=cmus
PKG_VERSION:=2.10.0
PKG_RELEASE:=$(AUTORELEASE)

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://github.com/cmus/cmus/archive/v$(PKG_VERSION)
PKG_HASH:=ff40068574810a7de3990f4f69c9c47ef49e37bd31d298d372e8bcdafb973fff

PKG_MAINTAINER:=Peter Wagner <tripolar@gmx.at>
PKG_LICENSE:=GPL-2.0-or-later
PKG_LICENSE_FILES:=COPYING

PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/nls.mk

define Package/cmus/Default
  SECTION:=sound
  CATEGORY:=Sound
  URL:=https://cmus.github.io/
endef

define Package/cmus
  $(call Package/cmus/Default)
  TITLE:=C* Music Player
  DEPENDS:= \
	+libpthread +librt +libncursesw $(ICONV_DEPENDS) \
	+alsa-lib +libflac +libmad +libvorbisidec
endef

define Package/cmus-plugin-ffmpeg
  $(call Package/cmus/Default)
  TITLE:=cmus ffmpeg plugin
  DEPENDS:= \
	cmus \
	+libffmpeg-full
endef

define Package/cmus-plugin-pulse
  $(call Package/cmus/Default)
  TITLE:=cmus pulseaudio plugin
  DEPENDS:= \
	cmus \
	+pulseaudio-daemon
endef

define Package/cmus-plugin-oss
  $(call Package/cmus/Default)
  TITLE:=cmus oss plugin
  DEPENDS:= \
	cmus
endef

define Package/cmus/Default/description
  C* Music Player is a modular and very configurable ncurses-based
  audio player. It has some interesting features like configurable
  colorscheme, mp3 and ogg streaming, it can be controlled with an
  UNIX socket, filters, album/artists sorting and a vi-like
  configuration interface.
endef

define Package/cmus/description
  $(call Package/cmus/Default/description)
endef

define Package/cmus-plugin-ffmpeg/description
  $(call Package/cmus/Default/description)
  This package adds the FFmpeg plugin.
endef

define Package/cmus-plugin-pulse/description
  $(call Package/cmus/Default/description)
  This package adds the PulseAudio plugin.
endef

define Package/cmus-plugin-oss/description
  $(call Package/cmus/Default/description)
  This package adds the OSS plugin.
endef

define Build/Configure
	# this is *NOT* GNU configure
	( cd $(PKG_BUILD_DIR); \
		CROSS="$(TARGET_CROSS)" \
		CFLAGS="$(TARGET_CFLAGS) $(TARGET_CPPFLAGS) -I$(ICONV_PREFIX)/include" \
		LDFLAGS="$(TARGET_LDFLAGS) -Wl,-rpath-link=$(STAGING_DIR)/usr/lib -L$(ICONV_PREFIX)/lib" \
		PKG_CONFIG_PATH="$(STAGING_DIR)/usr/lib/pkgconfig" \
		PKG_CONFIG_LIBDIR="$(STAGING_DIR)/usr/lib/pkgconfig" \
		./configure \
		prefix="/usr" \
		CONFIG_AAC=n \
		CONFIG_ALSA=a \
		CONFIG_AO=n \
		CONFIG_ARTS=n \
		CONFIG_CDDB=n \
		CONFIG_CDIO=n \
		CONFIG_COREAUDIO=n \
		CONFIG_CUE=y \
		CONFIG_DISCID=n \
		CONFIG_FFMPEG=a \
		CONFIG_FLAC=a \
		CONFIG_JACK=n \
		CONFIG_MAD=a \
		CONFIG_MIKMOD=n \
		CONFIG_BASS=n \
		CONFIG_MODPLUG=n \
		CONFIG_MP4=n \
		CONFIG_MPC=n \
		CONFIG_MPRIS=n \
		CONFIG_OPUS=n \
		CONFIG_OSS=a \
		CONFIG_PULSE=a \
		CONFIG_ROAR=n \
		CONFIG_SAMPLERATE=n \
		CONFIG_SNDIO=n \
		CONFIG_SUN=n \
		CONFIG_TREMOR=y \
		CONFIG_VORBIS=a \
		CONFIG_VTX=n \
		CONFIG_WAVEOUT=n \
		CONFIG_WAVPACK=n \
		CONFIG_WAV=y \
		DEBUG=0 \
	)
endef

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR) \
		DESTDIR="$(PKG_INSTALL_DIR)" \
		V=$(V) \
		install
endef

define Package/cmus/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_DIR) $(1)/usr/lib/cmus/ip
	$(INSTALL_DIR) $(1)/usr/lib/cmus/op
	$(INSTALL_DIR) $(1)/usr/share/cmus
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/cmus $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/lib/cmus/ip/{cue,flac,mad,vorbis,wav}.so $(1)/usr/lib/cmus/ip/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/lib/cmus/op/alsa.so $(1)/usr/lib/cmus/op/
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/share/cmus/{rc,default.theme} $(1)/usr/share/cmus/
endef

define Package/cmus-plugin-ffmpeg/install
	$(INSTALL_DIR) $(1)/usr/lib/cmus/ip
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/lib/cmus/ip/ffmpeg.so $(1)/usr/lib/cmus/ip/
endef

define Package/cmus-plugin-pulse/install
	$(INSTALL_DIR) $(1)/usr/lib/cmus/op
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/lib/cmus/op/pulse.so $(1)/usr/lib/cmus/op/
endef

define Package/cmus-plugin-oss/install
	$(INSTALL_DIR) $(1)/usr/lib/cmus/op
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/lib/cmus/op/oss.so $(1)/usr/lib/cmus/op/
endef


$(eval $(call BuildPackage,cmus))
$(eval $(call BuildPackage,cmus-plugin-ffmpeg))
$(eval $(call BuildPackage,cmus-plugin-pulse))
$(eval $(call BuildPackage,cmus-plugin-oss))

