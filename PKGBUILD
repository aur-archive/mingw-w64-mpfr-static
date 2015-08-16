pkgname=mingw-w64-mpfr-static
pkgver=3.1.2
pkgrel=1
pkgdesc="Multiple-precision floating-point library (mingw-w64, static)"
arch=(any)
url="http://www.mpfr.org"
license=("LGPL")
makedepends=(mingw-w64-gcc)
depends=(mingw-w64-crt "mingw-w64-gmp-static>=5.0")
conflicts=(mingw-w64-mpfr)
provides=(mingw-w64-mpfr)
options=(!libtool !strip !buildflags)
source=("http://www.mpfr.org/mpfr-current/mpfr-${pkgver}.tar.xz")
md5sums=('e3d203d188b8fe60bb6578dd3152e05c')

_architectures="i686-w64-mingw32 x86_64-w64-mingw32"

build() {
  for _arch in ${_architectures}; do
    unset LDFLAGS
    mkdir -p "${srcdir}/${pkgname}-${pkgver}-build-${_arch}"
    cd "${srcdir}/${pkgname}-${pkgver}-build-${_arch}"
    ${srcdir}/${pkgname#mingw-w64-}-${pkgver}/configure \
      --prefix=/usr/${_arch} \
      --build=$CHOST \
      --host=${_arch} \
      --enable-thread-safe
    make
  done
}

package() {
  for _arch in ${_architectures}; do
    cd "${srcdir}/${pkgname}-${pkgver}-build-${_arch}"
    make DESTDIR="$pkgdir" install
    find "$pkgdir/usr/${_arch}" -name '*.a' | xargs -rtl1 ${_arch}-strip -g
    rm -r "$pkgdir/usr/${_arch}/share"
  done
}
