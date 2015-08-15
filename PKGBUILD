# Maintainer: Yanchenko Igor <yanchenko.igor@gmail.com>
pkgname=flashcache-git
pkgver=20120928
pkgrel=1
pkgdesc="FlashCache is a general purpose writeback block cache for Linux."
arch=('x86_64')
url="https://github.com/facebook/flashcache"
license=('GPL')
makedepends=('git' 'linux-headers')
source=('flashcache_install'
        'flashcache_hook'
        'flashcache.conf')
md5sums=('0a2bba537e0fa17634d6202b968e65f0'
         'd93aab380b3c25bcf8adff2a36d9516a'
         '975a5193d561bf9a42b8cb42cca4764c')
backup=("etc/flashcache.conf")
install="flashcache.install"

_gitroot=git://github.com/facebook/flashcache.git
_gitname=flashcache
KERNEL_SOURCE_VERSION=$(uname -r)

build() {
  cd "$srcdir"
  msg "Connecting to GIT server...."

  if [[ -d "$_gitname" ]]; then
    cd "$_gitname" && git pull origin
    msg "The local files are updated."
  else
    git clone "$_gitroot" "$_gitname"
  fi

  msg "GIT checkout done or server timeout"
  msg "Starting build..."

  rm -rf "$srcdir/$_gitname-build"
  git clone "$srcdir/$_gitname" "$srcdir/$_gitname-build"
  cd "$srcdir/$_gitname-build"

  #
  # BUILD HERE
  #
  make KERNEL_TREE=/usr/src/linux-${KERNEL_SOURCE_VERSION}
}

package() {
  cd "$srcdir/$_gitname-build"
  install -D -m644 ../flashcache.conf $pkgdir/etc/flashcache.conf
  install -D -m644 "${srcdir}/flashcache_install" "${pkgdir}/usr/lib/initcpio/install/flashcache"
  install -D -m644 "${srcdir}/flashcache_hook" "${pkgdir}/usr/lib/initcpio/hooks/flashcache"
  install -m 0755 -d $pkgdir/lib/modules/$KERNEL_SOURCE_VERSION/extra/flashcache/
  install -m 0755 src/flashcache.ko $pkgdir/lib/modules/$KERNEL_SOURCE_VERSION/extra/flashcache/
  make DESTDIR="$pkgdir/" -C src/utils install

}

# vim:set ts=2 sw=2 et:
