# This file is part of BlackArch Linux ( https://www.blackarch.org/ ).
# See COPYING for license details.

pkgname=cubicsdr
pkgver=0.2.7
pkgrel=1
pkgdesc='Cross-Platform Software-Defined Radio Application.'
arch=('x86_64')
groups=('blackarch' 'blackarch-radio')
url='https://github.com/cjcliffe/CubicSDR'
license=('GPL')
depends=('libpulse' 'wxgtk2-dev' 'wxgtk-common-dev' 'soapysdr-git' 'liquid-dsp-git')
optdepends=(
    'fftw: FFTW support'
    'soapyrtlsdr-git: support for RTL-SDR (RTL2832U) dongles'
    'soapysdrplay-git: support for Airspy R2 and Airspy Mini'
    'soapyhackrf-git: support for HackRF'
    'soapylms7: support for LimeSDR'
    'soapyosmo-git: support for MiriSDR and RDSpace'
    'soapyplutosdr-git: support for PlutoSDR'
    'soapyremote-git: use any SoapySDR device remotely over network'
    'hamlib: hamlib support'
)
makedepends=('git' 'automake' 'cmake' 'libicns')
#install="$pkgname.install"
source=(cubicsdr::"git+https://github.com/cjcliffe/$pkgname.git")
sha512sums=('SKIP')

prepare() {
  cd "$pkgname"

  printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

build() {
  cd "$pkgname"
  mkdir -p build
  cd build

  # Determine if hamlib should be enabled
  if rigctl -V &>/dev/null; then
      _hamlib='-DUSE_HAMLIB=1';
      msg2 "hamlib found and enabled!"
  else
      _hamlib='';
      msg2 "hamlib not found"
  fi

  cmake ../ -DCMAKE_INSTALL_PREFIX:PATH=/usr \
      -DCMAKE_BUILD_TYPE=Release \
      -DwxWidgets_CONFIG_EXECUTABLE=$(which wx-config) \
      $_hamlib
  make
}

package() {
  cd "$pkgname/build"

  make DESTDIR="$pkgdir" install

  install -Dm 644 -t "$pkgdir/usr/share/doc/$pkgname/" doc/* *.md

  # Only install license if required according to
  # https://wiki.archlinux.org/index.php/PKGBUILD#license
  install -Dm 644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}

