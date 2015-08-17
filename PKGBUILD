# $Id: PKGBUILD 85263 2013-02-28 10:38:27Z spupykin $
# Maintainer: Sergej Pupykin <pupykin.s+arch@gmail.com>
# Contributor: Sergej Pupykin <pupykin.s+arch@gmail.com>

pkgname=rusxmms
pkgver=1.2.11_csa43
_xmmsver=1.2.11
_csaver=csa43
pkgrel=3
pkgdesc="XMMS with librcc"
arch=(i686 x86_64)
license=(GPL)
url="http://rusxmms.sourceforge.net/"
depends=(libsm libxxf86vm zlib gtk libvorbis alsa-lib libgl librcc librcd openssl)
makedepends=(mesa patch)
provides=(xmms)
conflicts=(xmms)
options=('!libtool' '!distcc')
source=(http://xmms.org/files/1.2.x/xmms-${_xmmsver}.tar.bz2 \
	http://dside.dyndns.org/files/rusxmms/RusXMMS2-${_csaver}.tar.bz2)
md5sums=('f3e6dbaf0b3f571a532ab575656be506'
         '8f387dd2e5c95f8730979e09687b6e02')

build() {
  cd "${srcdir}"/xmms-${_xmmsver}
  sed -i 's/AM_CONFIG_HEADER/AC_CONFIG_HEADER/g' configure.in libxmms/configure.in

  ln -s "$srcdir"/RusXMMS2 "$srcdir"/xmms-${_xmmsver}/RusXMMS2
  (cd "$srcdir"/xmms-${_xmmsver}/RusXMMS2 && ./apply.sh)

  mv "$srcdir"/RusXMMS2/source/* "$srcdir"/xmms-${_xmmsver}/libxmms/
  autoconf
  sed -i 's/unicode.c//g' Input/mpg123/Makefile.in
  sed -i 's/unicode.lo//g' Input/mpg123/Makefile.in

  (cd libxmms && aclocal && automake && autoconf)

  case $CARCH in
    x86_64)
	./configure --prefix=/usr --disable-mikmod --disable-simd
	;;
    i686)
	./configure --prefix=/usr --disable-mikmod --enable-simd --disable-vorbis --disable-vorbistest
	;;
    *)
	return 1
	;;
  esac

  make
}

package(){
  cd "${srcdir}"/xmms-${_xmmsver}
  make DESTDIR="$pkgdir" install

  mkdir -p "$pkgdir"/usr/share/{applications,pixmaps}
  install -m 644 xmms/xmms.desktop "$pkgdir"/usr/share/applications
  install -m 644 xmms/xmms_mini.xpm "$pkgdir"/usr/share/pixmaps/xmms.xpm
  # don't want wmxmms
  rm -rf "$pkgdir"/usr/bin/wmxmms "$pkgdir"/usr/share/xmms
  rm -f "$pkgdir"/usr/share/man/man1/{gnomexmms.1,wmxmms.1}
}
