# Maintainer: tsp <patrictroy at gmail dot com>

#Configuration:
_fragmant="${FRAGMENT:-#branch=dev}"
pkgname=rizin-git
pkgver=0.4.0.r26837.d5dea0a088
pkgrel=1
epoch=1
pkgdesc="Open-source tools to disasm, debug, analyze and manipulate binary files"
arch=('i686' 'x86_64')
url="https://rizin.re/"
license=('GPL3' 'LGPL3')
makedepends=('git' 'meson' 'ninja' 'python-pyaml')
depends=('capstone' 'lz4' 'file' 'libzip' 'xxhash' 'libuv' 'tree-sitter')
depends=('sh' 'capstone' 'libcapstone.so' 'openssl' 'libzip' 'zlib'
         'lz4' 'xxhash' 'libxxhash.so' 'file' 'libuv' 'tree-sitter'
         'libtree-sitter.so')
provides=("rizin=${pkgver%.r*}")
conflicts=('rizin')
source=("$pkgname::git+https://github.com/rizinorg/rizin.git${_fragmant}")
md5sums=('SKIP')

pkgver () {
  cd ${pkgname}
  printf "%s.r%s.%s" "$(grep -oP "^  version: 'v\K[0-9.]*(?=(-git)',$)" meson.build)" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

prepare() {
  cd ${pkgname}
  meson subprojects download
}

build() {
  # this is actually needed to prevent linking against old system-wide r2 libs
  # you can comment this out, if you build in a clean environment
  export PKG_CONFIG_PATH="${srcdir}/${pkgname}/pkgcfg:${PKG_CONFIG_PATH}"

  cd ${pkgname}
  arch-meson build                      \
    -D use_sys_capstone=enabled         \
    -D use_sys_magic=enabled            \
    -D use_sys_libzip=enabled           \
    -D use_sys_zlib=enabled             \
    -D use_sys_lz4=enabled              \
    -D use_sys_xxhash=enabled           \
    -D use_sys_openssl=enabled          \
    -D use_sys_tree_sitter=enabled      \
    -D use_libuv=true                   \
    -D packager="AUR"                   \
    -D packager_version="${pkgver}-git"
  ninja -C build
}

package() {
  cd ${pkgname}
  DESTDIR="${pkgdir}" ninja -C build install
  install -dm644 "${pkgdir}/usr/share/doc/rizin"
  cp -r doc/* "${pkgdir}/usr/share/doc/rizin"
}
