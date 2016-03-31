# Maintainer: Mike Williamson <mike at korora dot ca>
#
# based on:
# - https://aur.archlinux.org/packages/arangodb
# - https://aur.archlinux.org/packages/arangodb-git

pkgname=arangodb-git
pkgver="r1.62dd82b"
pkgver() {
  cd "$srcdir/arangodb"
  printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}
pkgrel=1
pkgdesc="A multi-model NoSQL database, combining key-value, document and graph data models."
arch=("i686" "x86_64" "armv7l" "armv7h")
url="https://www.arangodb.com/"
license=('APACHE')
depends=("glibc" "gcc-libs" "openssl" "readline" "systemd")
makedepends=("cmake binutils python2 go")
provides=("arangodb-git")
conflicts=("arangodb-latest" "arangodb-git")
backup=('etc/arangodb/arangob.conf'
'etc/arangodb/arangodump.conf'
'etc/arangodb/arangorestore.conf'
'etc/arangodb/arangod.conf'
'etc/arangodb/arangoimp.conf'
'etc/arangodb/arangosh.conf'
'etc/arangodb/arango-dfdb.conf'
'etc/arangodb/foxx-manager.conf'
)
options=()
install=arangodb.install
source=("https://github.com/arangodb/arangodb.git" "arangodb.service")
sha256sums=('SKIP'
'd4bf2ba13ac8a66cec3c8c94bc74c474513ef4aa3973f01d0b3a9fbba7596123')

build() {
  msg2 "Symlinking 'python' to python2."
  ln -s -f /usr/bin/python2 python
  export PATH="`pwd`:$PATH"
  export LD="ld.gold"

  msg2 "Configuring ArangoDB."
  cd $srcdir/arangodb
  [ -d build ] || mkdir build && cd build
  cmake -DCMAKE_INSTALL_PREFIX="/usr" ..
  msg2 "Building ArangoDB."
  make -j $(nproc)
}

package() {
  msg2 "Preparing ArangoDB."
  mkdir p $pkgdir/usr/libexec/arangodb
  cp -R $srcdir/arangodb/build/etc $pkgdir
  cp -R $srcdir/arangodb/build/var $pkgdir
  cp -R $srcdir/arangodb/build/bin/etcd-arango $pkgdir/usr/libexec/arangodb
  cp -R $srcdir/arangodb/build/bin $pkgdir/usr
  cp -R $srcdir/arangodb/build/lib $pkgdir/usr
  mkdir -p $pkgdir/usr/share/doc/arangodb
  mkdir -p $pkgdir/usr/share/arangodb/js
  cp -R $srcdir/arangodb/js/* $pkgdir/usr/share/arangodb/js
  cp -R $srcdir/arangodb/Documentation/* $pkgdir/usr/share/doc/arangodb/

  msg2 "Preparing systemd service."
  mkdir -p $pkgdir/usr/lib/systemd/system
  cp $srcdir/arangodb.service $pkgdir/usr/lib/systemd/system/
}
