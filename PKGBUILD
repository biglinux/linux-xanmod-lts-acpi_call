# Maintainer: Philip Müller <philm[at]manjaro[dot]org>
# Maintainer: Bernhard Landauer <bernhard@manjaro.org>

# Arch credits:
# Contributor: Maxime Gauduin <alucryd@gmail.com>
# Contributor: mortzu <me@mortzu.de>
# Contributor: fnord0 <fnord0@riseup.net>

_linuxprefix=linux-xanmod-lts

_module=acpi_call
pkgname="${_linuxprefix}-${_module}"
pkgver=1.2.2
pkgrel=66341
pkgdesc='A linux kernel module that enables calls to ACPI methods through /proc/acpi/call'
arch=('x86_64')
url="https://github.com/nix-community/acpi_call"
license=('GPL')
groups=("${_linuxprefix}-extramodules")
depends=("${_linuxprefix}")
makedepends=("${_linuxprefix}-headers" "acpi_call-dkms=$pkgver")
provides=("${_module}")
source=("${_module}-$pkgver.tar.gz::${url}/archive/refs/tags/v$pkgver.tar.gz")
sha256sums=('8b1902a94395c2fa5a97f81c94868a9cbc46a48e12309ad01626439bde96f1d9')

build() {
  _kernver="$(cat /usr/src/${_linuxprefix}/version)"
  _kernver="$(cat /usr/src/${_linuxprefix}/version)"

  fakeroot dkms build --dkmstree "$srcdir" -m "${_module}/$pkgver" -k "${_kernver}"
}

package() {
  _kernver="$(cat /usr/src/${_linuxprefix}/version)"

  install -Dm644 "${_module}/${pkgver}/${_kernver}/$CARCH/module"/* -t \
    "$pkgdir/usr/lib/modules/${_kernver}/extramodules/"

  # compress each module individually
  find "$pkgdir" -name '*.ko' -exec xz -T1 {} +

  echo acpi_call | install -Dm644 /dev/stdin "$pkgdir/usr/lib/modules-load.d/$pkgname.conf"

  install -d "$pkgdir/usr/share"
  cp -a "/usr/share/${_module}" "$pkgdir/usr/share/$pkgname"
}
