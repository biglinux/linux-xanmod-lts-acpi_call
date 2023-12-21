# Maintainer: Bernhard Landauer <bernhard@manjaro.org>
# Maintainer: Philip Müller <philm[at]manjaro[dot]org>
# Archlinux credits:
# Contributor: Maxime Gauduin <alucryd@gmail.com>
# Contributor: mortzu <me@mortzu.de>
# Contributor: fnord0 <fnord0@riseup.net>

_linuxprefix=linux-xanmod-lts
_extramodules=$(find /usr/lib/modules -type d -iname 6.1.69*xanmod* | rev | cut -d "/" -f1 | rev)
pkgname=$_linuxprefix-acpi_call
_pkgname=acpi_call
pkgver=1.2.2
pkgrel=61691
pkgdesc='A linux kernel module that enables calls to ACPI methods through /proc/acpi/call'
arch=('x86_64')
url="https://github.com/nix-community/$_pkgname"
license=('GPL')
depends=("$_linuxprefix")
makedepends=("$_linuxprefix-headers")
provides=("$_pkgname")
groups=("$_linuxprefix-extramodules")
install=$_pkgname.install
source=("$_pkgname-$pkgver.tar.gz::${url}/archive/refs/tags/v$pkgver.tar.gz")
sha256sums=('8b1902a94395c2fa5a97f81c94868a9cbc46a48e12309ad01626439bde96f1d9')

build() {
  _kernver=$(find /usr/lib/modules -type d -iname 6.1.69*xanmod* | rev | cut -d "/" -f1 | rev)
  cd "${_pkgname}-${pkgver}"
  make KVER="${_kernver}"
}

package() {
  cd "${_pkgname}-${pkgver}"

  install -dm 755 "${pkgdir}"/usr/lib/{modules/${_extramodules},modules-load.d}
  install -m 644 acpi_call.ko "${pkgdir}"/usr/lib/modules/${_extramodules}/
  gzip "${pkgdir}"/usr/lib/modules/${_extramodules}/acpi_call.ko
  echo acpi_call > "${pkgdir}"/usr/lib/modules-load.d/${pkgname}.conf

  install -dm 755 "${pkgdir}"/usr/share/${pkgname}
  cp -dr --no-preserve='ownership' {examples,support} "${pkgdir}"/usr/share/${pkgname}/

  sed -i "s/EXTRAMODULES=.*/EXTRAMODULES=$_extramodules/" \
    "$startdir/acpi_call.install"
}
