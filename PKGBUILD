# Maintainer: Thomas Jost <schnouki@schnouki.net>
# Contributor: Andreas Radke <andyrtr@archlinux.org>
# Contributor: Art Gramlich <art@gramlich-net.com>

pkgname=icu52
_pkgname=icu
pkgver=52.1
pkgrel=1
pkgdesc="International Components for Unicode library"
arch=(i686 x86_64)
url="http://www.icu-project.org/"
license=('custom:"icu"')
depends=('gcc-libs>=4.7.1-5' 'sh')
provides=("$_pkgname==$pkgver")
source=(http://download.icu-project.org/files/${_pkgname}4c/${pkgver}/${_pkgname}4c-${pkgver//./_}-src.tgz
	icu.8198.revert.icu5431.patch)
md5sums=('9e96ed4c1d99c0d14ac03c140f9f346c'
         'ebd5470fc969c75e52baf4af94a9ee82')

prepare() {
  cd icu/source
  # fix Malayalam encoding https://bugzilla.redhat.com/show_bug.cgi?id=654200
  patch -Rp3 -i ${srcdir}/icu.8198.revert.icu5431.patch
}

build() {
  cd icu/source
  ./configure --prefix=/usr \
	--sysconfdir=/etc \
	--mandir=/usr/share/man \
	--sbindir=/usr/bin
  make
}

check() {
  cd icu/source
  make -k check # passes all
}

package() {
  cd icu/source
  make -j1 DESTDIR=${pkgdir} install

  # Install license
  install -Dm644 ${srcdir}/icu/license.html ${pkgdir}/usr/share/licenses/${pkgname}/license.html

  # Only keep versioned libraries
  cd "$pkgdir/usr"
  for dir in bin include lib/pkgconfig share/man; do
    rm -rf $dir
  done
  cd lib
  rm *.so icu/{current,Makefile.inc,pkgdata.inc}
}
