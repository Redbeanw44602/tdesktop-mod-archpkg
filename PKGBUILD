# Maintainer: Sven-Hendrik Haase <svenstaro@archlinux.org>
# Contributor: hexchain <i@hexchain.org>

pkgname=telegram-desktop
pkgver=6.8.2
pkgrel=4
pkgdesc='Official Telegram Desktop client [MOD]'
arch=('x86_64')
url="https://desktop.telegram.org/"
license=('GPL-3.0-or-later WITH OpenSSL-exception')
depends=(
  'abseil-cpp'
  'ada'
  'ffmpeg'
  'glib2'
  'hicolor-icon-theme'
  'hunspell'
  'kcoreaddons'
  'libavif'
  'libdispatch'
  'libheif'
  'libjpeg-turbo'
  'libjxl'
  'libpipewire'
  'libxcb'
  'libxcomposite'
  'libxdamage'
  'libxext'
  'libxfixes'
  'libxkbcommon'
  'libxrandr'
  'libxtst'
  'lz4'
  'minizip'
  'openal'
  'openh264'
  'openssl'
  'pipewire'
  'protobuf'
  'qt6-base'
  'qt6-imageformats'
  'qt6-svg'
  'qt6-wayland'
  'rnnoise'
  'xxhash'
)
makedepends=(
  'boost'
  'boost-libs'
  'cmake'
  'git'
  'glib2-devel'
  'gobject-introspection'
  'gperf'
  'libtg_owt'
  'microsoft-gsl'
  'ninja'
  'python'
  'range-v3'
  'tl-expected'
)
optdepends=(
  'geoclue: geoinformation support'
  'crow-translate: translation provider'
  'webkit2gtk-4.1: embedded browser features provided by webkit2gtk-4.1'
  'webkitgtk-6.0: embedded browser features provided by webkitgtk-6.0 (Wayland only)'
  'xdg-desktop-portal: desktop integration'
)
_td_commit=49b3bcbb6bfebf2ed44dd9f25102d2e1a94a58c4
source=(
  "git+https://github.com/Redbeanw44602/tdesktop-mod.git#tag=v${pkgver}-mod"
  "git+https://github.com/tdlib/td.git#tag=${_td_commit}"
  tdesktop-fix-minizip-includes.patch
)
sha512sums=('d21ab5e47b6296409399ac40d52dfb4a2836aa20d96b744dc449d8dbf01d4bea94fbe1ef8d0da0f9517e0c0b4a1ea6f9afe8eed1c4e2741b5fc3296144ffc4a7'
            'f8f98b02b1c7d1ca9162c4867461605fa7a5ab449ac53701877f49ba393ff4a495a58984538fe3960c7090ab5b3749666b4d169058f5e40f8d35ea4c15aea8d5'
            'd9765588e92f154d83b95dc2840207bf22b26b6ca37b4d5cdfdb5e27a00c9e1ebcc9cd475a96bbcc5b02c24f6892320e009f843aa6b172a1820814b952a772eb')

prepare() {
  cd tdesktop-mod && git submodule update --init --recursive
  patch -Np1 -d Telegram/lib_base -i "$srcdir"/tdesktop-fix-minizip-includes.patch
}

build() {
  cmake -S td -B td/build \
    -DCMAKE_BUILD_TYPE=None \
    -DCMAKE_INSTALL_PREFIX="$PWD/td/install" \
    -Wno-dev \
    -DTD_E2E_ONLY=ON
  cmake --build td/build
  cmake --install td/build

  # Turns out we're allowed to use the official API key that telegram uses for
  # their snap builds:
  # https://github.com/telegramdesktop/tdesktop/blob/8fab9167beb2407c1153930ed03a4badd0c2b59f/snap/snapcraft.yaml#L87-L88
  # Thanks @primeos!
  cmake -B build -S tdesktop-mod -G Ninja \
    -DCMAKE_VERBOSE_MAKEFILE=ON \
    -DCMAKE_INSTALL_PREFIX="/usr" \
    -Dtde2e_DIR="$PWD/td/install/lib/cmake/tde2e" \
    -DCMAKE_BUILD_TYPE=Release \
    -DTDESKTOP_API_ID=611335 \
    -DTDESKTOP_API_HASH=d524b414d21f4d37f08684c1df41ac9c
  cmake --build build
}

package() {
  DESTDIR="$pkgdir" cmake --install build
}
