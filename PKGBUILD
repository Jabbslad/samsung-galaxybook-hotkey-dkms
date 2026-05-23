# Maintainer: local
#
# The samsung-galaxybook driver was authored and upstreamed by Joshua Grisham.
# See the original project/research repo for background and credits:
#   https://github.com/joshuagrisham/samsung-galaxybook-extras
#
# This package fetches a pinned Linux source version and applies a small
# temporary SAM0430 hotkey patch until the running kernel includes it.

pkgname=samsung-galaxybook-hotkey-dkms
_pkgbase=samsung-galaxybook-hotkey
pkgver=0.2
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
_linux_commit='028ef9c96e96197026887c0f092424679298aae8' # v7.0 source, before SAM0430 hotkey handling
source=(
  "samsung-galaxybook.c::https://raw.githubusercontent.com/torvalds/linux/${_linux_commit}/drivers/platform/x86/samsung-galaxybook.c"
  "firmware_attributes_class.h::https://raw.githubusercontent.com/torvalds/linux/${_linux_commit}/drivers/platform/x86/firmware_attributes_class.h"
  'sam0430-hotkeys.patch'
  'Makefile'
  'dkms.conf'
)
sha256sums=('SKIP' 'SKIP' 'SKIP' 'SKIP' 'SKIP')

prepare() {
  cd "${srcdir}"

  # makepkg symlinks sources into srcdir; patch refuses to modify symlinks.
  cp -fL samsung-galaxybook.c samsung-galaxybook.c.orig
  rm -f samsung-galaxybook.c
  cp samsung-galaxybook.c.orig samsung-galaxybook.c

  patch -Np0 -i sam0430-hotkeys.patch
}

package() {
  local dkmsdir="${pkgdir}/usr/src/${_pkgbase}-${pkgver}"
  install -dm755 "${dkmsdir}"
  install -m644 \
    "${srcdir}/samsung-galaxybook.c" \
    "${srcdir}/firmware_attributes_class.h" \
    Makefile \
    dkms.conf \
    "${dkmsdir}/"
}
