# Maintainer: Mike Williamson <mike at korora dot ca>
#
# based on:
# - https://aur.archlinux.org/packages/arangodb
# - https://aur.archlinux.org/packages/arangodb-git

pkgname=arangodb
pkgver=2015.05.25.ge1a7ae8
pkgrel=1
pkgdesc="A multi-model NoSQL database, combining key-value, document and graph data models."
arch=("i686" "x86_64" "armv7h")
url="https://www.arangodb.com/"
license=('APACHE')
depends=("glibc" "gcc-libs" "openssl" "readline" "systemd")
makedepends=("python2 go>=1.4")
provides=("arangodb-git")
conflicts=("arangodb-latest" "arangodb-git")
backup=()
options=()
install=arangodb.install
# TODO: Figure this out
# DLAGENTS=('shallow::/usr/bin/git clone --depth 1 %u')
source=("git+https://github.com/arangodb/arangodb.git" "arangodb.service")
sha256sums=('SKIP'
'd4bf2ba13ac8a66cec3c8c94bc74c474513ef4aa3973f01d0b3a9fbba7596123')

build() {
  msg2 "Symlinking 'python' to python2."
  ln -s -f /usr/bin/python2 python
  export PATH="`pwd`:$PATH"

  # TODO find out what good flags for production are.
  export CFLAGS="-g -O2"
  export CXXFLAGS="-g -O2 -Wno-error=strict-overflow"

  msg2 "Configuring ArangoDB."
  cd $srcdir/arangodb
  make setup
  ./configure \
    --prefix=/usr \
    --sbindir=/usr/bin \
    --sysconfdir=/etc \
    --localstatedir=/var \
    --enable-all-in-one-etcd \
    --enable-armv7

  msg2 "Building ArangoDB."
  make -j $(nproc)
}

package() {
  msg2 "Preparing ArangoDB."
  cd $srcdir/arangodb
  make DESTDIR="$pkgdir/" install
  mkdir -p $pkgdir/usr/share/doc/arangodb
  cp -R $srcdir/arangodb/Documentation/* $pkgdir/usr/share/doc/arangodb/

  msg2 "Preparing systemd service."
  mkdir -p $pkgdir/usr/lib/systemd/system
  cp $srcdir/arangodb.service $pkgdir/usr/lib/systemd/system/
}


pkgver() {
  cd "$srcdir/${_gitname}"
  git log -1 --format="%cd.g%h" --date=short | sed 's/-/./g'
}
