# Contributor: Graziano Giuliani <graziano.giuliani@poste.it>
# Contributor: Antonio Cervone <ant.cervone@gmail.com>

pkgname=eccodes
pkgver=2.17.0
_attnum=45757960
pkgrel=1
pkgdesc="ECMWF decoding library for GRIB, BUFR and GTS"
arch=('i686' 'x86_64')
url="https://software.ecmwf.int/wiki/display/ECC/ecCodes+Home"
license=('Apache')
depends=('libpng' 'netcdf')
optdepends=('libaec: for compression' 'jasper: as an alternative to openjpeg')
makedepends=('gcc-fortran' 'python' 'python-numpy' 'cmake')
conflicts=('grib_api' 'libbufr-ecmwf')
source=(http://software.ecmwf.int/wiki/download/attachments/${_attnum}/${pkgname}-${pkgver}-Source.tar.gz)
md5sums=('36a8c822e9a5eb0d70790354ae7fdd12')

build() {
  cd "$srcdir"/${pkgname}-${pkgver}-Source

  # sed -i 's/image.inmem_.*=.*1;//' src/grib_jasper_encoding.c

  [ -x /usr/bin/aec ] && has_aec=1 || has_aec=0

  mkdir -p build
  cd build
  cmake \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DCMAKE_BUILD_TYPE=production \
    -DCMAKE_INSTALL_DATAROOTDIR=/usr/share/$pkgname/definitions \
    -DCMAKE_INSTALL_DATADIR=/usr/share -DENABLE_AEC=$has_aec \
    -DENABLE_PNG=1 \
    -DENABLE_ECCODES_THREADS=1 \
    -DOPENJPEG_INCLUDE_DIR=`pkg-config --variable=includedir libopenjpeg` \
    -DPYTHON_EXECUTABLE=/usr/bin/python3 \
    ..

  make
}

package() {
  cd "$srcdir"/${pkgname}-${pkgver}-Source/build
  make DESTDIR="$pkgdir" install
}
