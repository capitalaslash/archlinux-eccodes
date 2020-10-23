# Contributor: Graziano Giuliani <graziano.giuliani@poste.it>
# Contributor: Antonio Cervone <ant.cervone@gmail.com>

pkgname=eccodes
pkgver=2.19.0
_attnum=45757960
pkgrel=1
pkgdesc="ECMWF decoding library for GRIB, BUFR and GTS"
arch=('i686' 'x86_64')
url="https://software.ecmwf.int/wiki/display/ECC/ecCodes+Home"
license=('Apache')
depends=(libpng netcdf ksh)
optdepends=('libaec: for compression' 'jasper: as an alternative to openjpeg')
makedepends=('gcc-fortran' 'python' 'python-numpy' 'cmake')
conflicts=('grib_api' 'libbufr-ecmwf')
source=(http://software.ecmwf.int/wiki/download/attachments/${_attnum}/${pkgname}-${pkgver}-Source.tar.gz)
sha256sums=('a1d080aed1b17a9d4e3aecccc5a328c057830cd4d54f451f5498b80b24c46404')

build() {
  # sed -i 's/image.inmem_.*=.*1;//' src/grib_jasper_encoding.c

  [ -x /usr/bin/aec ] && has_aec=1 || has_aec=0

  cmake \
    -B build \
    -S "${pkgname}-${pkgver}-Source" \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DCMAKE_BUILD_TYPE=production \
    -DCMAKE_INSTALL_DATAROOTDIR=/usr/share/$pkgname/definitions \
    -DCMAKE_INSTALL_DATADIR=/usr/share -DENABLE_AEC=$has_aec \
    -DCMAKE_Fortran_FLAGS="-fallow-argument-mismatch" \
    -DENABLE_PNG=1 \
    -DENABLE_ECCODES_THREADS=1 \
    -DOPENJPEG_INCLUDE_DIR=`pkg-config --variable=includedir libopenjpeg` \
    -DPYTHON_EXECUTABLE=/usr/bin/python3 \
    ..

  make -C build
}

package() {
  make -C build DESTDIR="$pkgdir" install
}

