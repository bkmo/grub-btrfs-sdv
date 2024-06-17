# Maintainer dalto <dalto at fastmail dot com>
# Contributor: Joona P <jonppep at gmail dot com>
# Contributor: Carsten Feuls <archlinux@carstenfeuls.de>
# Contributor: Anty <anty_fab at hotmail dot fr>
# Contributor: Evan Anderson <evananderson@thelinuxman.us>

pkgname=grub-btrfs-sdv
pkgver=4.13.1
pkgrel=2
pkgdesc="Include btrfs snapshots at boot options (grub menu)"
arch=('any')
url="https://github.com/Antynea/grub-btrfs"
license=('GPL3')
depends=('grub' 'btrfs-progs' 'bash' 'gawk')
makedepends=('git')
conflicts=('grub-btrfs' 'gub-btrfs-git')
provides=('grub-btrfs')
source=('git+https://github.com/bkmo/grub-btrfs-sdv')
md5sums=('SKIP')

prepare() {
  cd $pkgname

  git cherry-pick -n b2a3a2343cfb654950aba4a81ce50cf1b2eab962

  git cherry-pick -n 208c679ac5064abd0e5e65d5d6267c678b114398

  git cherry-pick -n 2054eb53d9d6734ceedae5a81c62b4195097114f

  git cherry-pick -n 7ba2c590303ca98e5fa792f4ea1026957434a174

}

package() {
	cd "$pkgname"
	make DESTDIR="${pkgdir}" INITCPIO=true GRUB_UPDATE_EXCLUDE=true install
}
