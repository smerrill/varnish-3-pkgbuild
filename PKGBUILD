# Maintainer: Jaroslav Lichtblau <dragonlord@aur.archlinux.org>
# Contributor: Douglas Soares de Andrade
# Contributor: Roberto Alsina <ralsina@kde.org>
# Contributor: Steven Merrill <steven.merrill@gmail.com>

pkgname=varnish-git
pkgver=20110606
pkgrel=1
pkgdesc="High-performance HTTP accelerator"
arch=('i686' 'x86_64')
url="http://www.varnish-cache.org/"
license=('BSD')
depends=('ncurses' 'pcre' 'groff' 'libxslt')
backup=('etc/varnish.conf')
provides=('varnish')
conflicts=('varnish')
install=varnish.install
options=('!libtool')

_gitroot="git://git.varnish-cache.org/varnish-cache"
_gitname="varnish"

build() {
  cd "$srcdir"
  msg "Connecting to git server...."

  if [[ -d $_gitname ]] ; then
    cd $_gitname && git pull origin
    msg "The local files are updated."
  else
    git clone $_gitroot $_gitname
  fi

  msg "git checkout done or server timeout"
  msg "Starting make..."

  rm -rf "$srcdir/$_gitname-build"
  git clone "$srcdir/$_gitname" "$srcdir/$_gitname-build"
  cd "$srcdir/$_gitname-build"

  ./autogen.sh
  ./configure --prefix=/usr --sysconfdir=/etc --localstatedir=/var
  make
}

package() {
  cd "$srcdir/$_gitname-build"

  make DESTDIR=${pkgdir} install

  install -d ${pkgdir}/var/$pkgname/$(hostname)
  install -D -m755 ${srcdir}/$pkgname.init ${pkgdir}/etc/rc.d/$pkgname
  install -D -m755 ${srcdir}/$pkgname.runit ${pkgdir}/etc/sv/$pkgname/run
  install -D -m755 ${srcdir}/$pkgname.log.runit ${pkgdir}/etc/sv/$pkgname/log/run
  install -D -m755 ${srcdir}/$pkgname.conf ${pkgdir}/etc/$pkgname.conf

#license
  install -D -m644 LICENSE ${pkgdir}/usr/share/licenses/$pkgname/LICENSE
}
