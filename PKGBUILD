# Maintainer: Sven-Hendrik Haase <svenstaro@archlinux.org>
# Contributor: hexchain <i@hexchain.org>

pkgname=telegram-desktop
pkgver=7.0.3
pkgrel=1
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
  'qt6-shadertools'
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
  'webkit2gtk-4.1: embedded browser features provided by webkit2gtk-4.1 (gtk3)'
  'webkitgtk-6.0: embedded browser features provided by webkitgtk-6.0 (gtk4)'
  'xdg-desktop-portal: desktop integration'
)
_td_commit=51743dfd01dff6179e2d8f7095729caa4e2222e9
source=(
  "git+https://github.com/Redbeanw44602/tdesktop-mod.git#tag=v${pkgver}-mod"
  "git+https://github.com/tdlib/td.git#commit=${_td_commit}"
)
sha512sums=('fda9797636481344258d8e76b0c7c4c15c17f158a7746cf98e8fa657a33fbad2848311d4f5d524f9f942370b224c43705571e7135c3baa5985038b911a35af05'
            'd622b8f3580ee49415546d025c4ba45f5b2de50b315fc379dc57c0427c5f815c7cc3820cca937c12182ee461641bb61f87ebc99b6c74a1a666cea9a08f0f41a0')

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
