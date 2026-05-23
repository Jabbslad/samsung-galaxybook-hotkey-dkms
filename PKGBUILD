# Maintainer: local
pkgname=samsung-galaxybook-hotkey-dkms
_pkgbase=samsung-galaxybook-hotkey
pkgver=0.1
pkgrel=1
pkgdesc='DKMS override for samsung_galaxybook SAM0430 ACPI hotkeys'
arch=('any')
url='https://github.com/joshuagrisham/samsung-galaxybook-extras'
license=('GPL-2.0-or-later')
depends=('dkms')
optdepends=(
  'linux-headers: headers for the stock Arch kernel'
  'linux-ptl-headers: headers for linux-ptl kernels'
)
source=(
  'samsung-galaxybook.c'
  'firmware_attributes_class.h'
  'Makefile'
  'dkms.conf'
)
sha256sums=('SKIP' 'SKIP' 'SKIP' 'SKIP')

package() {
  local dkmsdir="${pkgdir}/usr/src/${_pkgbase}-${pkgver}"
  install -dm755 "${dkmsdir}"
  install -m644 samsung-galaxybook.c firmware_attributes_class.h Makefile dkms.conf "${dkmsdir}/"
}
