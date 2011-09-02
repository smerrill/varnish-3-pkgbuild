# Maintainer: Jaroslav Lichtblau <dragonlord@aur.archlinux.org>
# Contributor: Douglas Soares de Andrade
# Contributor: Roberto Alsina <ralsina@kde.org>
# Contributor: Steven Merrill <steven.merrill@gmail.com>

pkgname=varnish
pkgver=3.0.1
pkgrel=1
pkgdesc="High-performance HTTP accelerator"
arch=('i686' 'x86_64')
url="http://www.varnish-cache.org/"
license=('BSD')
depends=('ncurses' 'pcre')
backup=('etc/varnish.conf')
install=$pkgname.install
options=('!libtool')
source=(http://repo.varnish-cache.org/source/$pkgname-$pkgver.tar.gz
        $pkgname.conf
        $pkgname.init
        $pkgname.runit
        $pkgname.log.runit)
sha256sums=('385807737777a392520c5334bb114f9fc0e5f8dbf5cc48b420baf5b0478eb279'
            '46701975e6d975966316b7d253ca6310544523a1ba57400441f538cb82e17962'
            'cd3b9cdaa9276aa314cedfbd57a3592163aeb002b4cdac6abfe523ae60dec88f'
            'a09837991e156cf60bd6d8d6ff9fc3fa279c6001e783e531a4d7364db7f3d099'
            '18318e89528f70292c703340c9e209d96edbe8c36cb9c59fe1fa08c48d9fc7dd')
build() {
  cd ${srcdir}/$pkgname-$pkgver

  ./configure --prefix=/usr --sysconfdir=/etc --localstatedir=/var
  make
}

package() {
  cd ${srcdir}/$pkgname-$pkgver

  make DESTDIR=${pkgdir} install

  install -d ${pkgdir}/var/$pkgname/$(hostname)
  install -D -m755 ${srcdir}/$pkgname.init ${pkgdir}/etc/rc.d/$pkgname
  install -D -m755 ${srcdir}/$pkgname.runit ${pkgdir}/etc/sv/$pkgname/run
  install -D -m755 ${srcdir}/$pkgname.log.runit ${pkgdir}/etc/sv/$pkgname/log/run
  install -D -m644 ${srcdir}/$pkgname.conf ${pkgdir}/etc/$pkgname/$pkgname.conf

#license
  install -D -m644 LICENSE ${pkgdir}/usr/share/licenses/$pkgname/LICENSE
}
