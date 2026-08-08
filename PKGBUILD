# Maintainer: Sven-Hendrik Haase <svenstaro@archlinux.org>
# Contributor: hexchain <i@hexchain.org>

pkgname=telegram-desktop
pkgver=7.0.9
pkgrel=2
pkgdesc='Official Telegram Desktop client [MOD]'
arch=('x86_64')
url="https://desktop.telegram.org/"
license=('GPL-3.0-or-later WITH OpenSSL-exception')
depends=(
  'abseil-cpp'
  'ada'
  'cmark-gfm'
  'ffmpeg'
  'glib2'
  'hicolor-icon-theme'
  'hunspell'
  'kcoreaddons'
  'libavif'
  'libfido2'
  'libdispatch'
  'libheif'
  'libjpeg-turbo'
  'libjxl'
  'libpipewire'
  'libsrtp'
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
  'qt6-shadertools'
  'gperf'
  'libtg_owt'
  'microsoft-gsl'
  'ninja'
  'python'
  'range-v3'
  'tl-expected'
  'vulkan-headers'
)
optdepends=(
  'geoclue: geoinformation support'
  'crow-translate: translation provider'
  'webkit2gtk-4.1: embedded browser features provided by webkit2gtk-4.1 (gtk3)'
  'webkitgtk-6.0: embedded browser features provided by webkitgtk-6.0 (gtk4)'
  'xdg-desktop-portal: desktop integration'
)
_td_commit=022d60202e446ad1287b9fb68e687c8a0760788b
source=(
  "git+https://github.com/Redbeanw44602/tdesktop-mod.git#tag=v${pkgver}-mod"
  "git+https://github.com/tdlib/td.git#commit=${_td_commit}"
)
sha512sums=('a78f74eb5eacdcc525f3fb0f35e19bfd94df5c122bf5c962ade586d0129bc5021df19ac430028f2f2ebbc6549cd0d7cf380a636659afda4ab409646adfcffc59'
            '45ef8f69708c46aef8e8d0301b8710467a208e43a9ebb5918152b49d24f9d6c8b69ca9a94f19c4e401f44e8d60706cd840832ce442ca1a839df942a7b88afde2')

prepare() {
  cd tdesktop-mod && git submodule update --init --recursive
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
