# Forked from https://github.com/Antynea/grub-btrfs

pkgname=grub-btrfs-sdv
pkgver=4.13.2
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
source=($url/archive/refs/tags/$pkgver.tar.gz)
md5sums=('SKIP')

package() {
 cd $pkgname-$pkgver
 install -Dm755 snapshot-detect -t "$pkgdir/usr/bin/"
 install -Dm644 snapshot-detect.desktop -t "$pkgdir/etc/xdg/autostart/"
 make DESTDIR="${pkgdir}" INITCPIO=true GRUB_UPDATE_EXCLUDE=true install
}
