# Forked from https://github.com/Antynea/grub-btrfs

pkgname=grub-btrfs-sdv
pkgver=4.13.1
pkgrel=1
pkgdesc="Include btrfs snapshots at boot options (grub menu), add systemd.volatile=state option"
arch=('any')
url="https://github.com/bkmo/grub-btrfs-sdv"
license=('GPL3')
depends=('grub' 'btrfs-progs' 'bash' 'gawk' 'inotify-tools')
optdepends=('snapper: For snapper support')
backup=('etc/default/grub-btrfs/config')
makedepends=('git')
conflicts=('grub-btrfs' 'gub-btrfs-git')
provides=('grub-btrfs')
source=('git+https://github.com/bkmo/grub-btrfs-sdv')
md5sums=('SKIP')

prepare() {
  cd $pkgname
  #cherry pick commits from Add-systemd-volatile branch
  git cherry-pick -n b2a3a2343cfb654950aba4a81ce50cf1b2eab962
  git cherry-pick -n 208c679ac5064abd0e5e65d5d6267c678b114398
  git cherry-pick -n 2054eb53d9d6734ceedae5a81c62b4195097114f
 }

package() {
 cd $pkgname
 # sed -i 's/#GRUB_BTRFS_SYSTEMD_VOLATILE=/GRUB_BTRFS_SYSTEMD_VOLATILE=/' config
 install -Dm755 snapshot-detect -t "$pkgdir/usr/bin/"
 install -Dm644 snapshot-detect.desktop -t "$pkgdir/etc/xdg/autostart/"
 make DESTDIR="${pkgdir}" INITCPIO=true GRUB_UPDATE_EXCLUDE=true install
}
