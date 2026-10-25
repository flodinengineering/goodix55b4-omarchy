pkgname=libfprint-goodix55b4-local
pkgver=1.94.6.r1.f7b06e4
pkgrel=4
pkgdesc='Experimental Goodix 27c6:55b4 driver with host finger detection'
arch=('x86_64')
url='https://github.com/jedbillyb/libfprint/tree/goodix-55b4-fixes'
license=('LGPL-2.1-or-later')
depends=('glib2' 'libgusb' 'openssl' 'opencv' 'pixman' 'glibc' 'gcc-libs')
makedepends=('meson' 'ninja' 'gobject-introspection')
provides=('libfprint=1.94.6' 'libfprint-2.so=2-64')
conflicts=('libfprint' 'libfprint-git')
options=('!debug')
source=('libfprint-f7b06e4.tar.gz' 'local-fixes.patch')
sha256sums=('9f5a4735de86e3483b34f62549e6ea069a3541a9260966319acf3ab739496f06' 'bc84159c87b3247127b151765271d32edc2d5fac77bea2214b622a85f920b112')

prepare() {
  patch -Np1 -i "$srcdir/local-fixes.patch"
}

build() {
  arch-meson . build -Ddrivers=goodixtls55x4 -Ddoc=false -Dinstalled-tests=false
  meson compile -C build
}

package() {
  DESTDIR="$pkgdir" meson install -C build --no-rebuild
}
