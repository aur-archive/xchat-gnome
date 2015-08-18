# $Id: PKGBUILD 112861 2011-03-07 10:29:01Z eric $
# Maintainer: Allan McRae <allan@archlinux.org>
# Contributor: Jan de Groot <jgc@archlinux.org>

pkgname=xchat-gnome
pkgver=0.26.1
pkgrel=5
pkgdesc="GNOME frontend to the popular X-Chat IRC client."
arch=('i686' 'x86_64')
license=('GPL')
url="http://xchat-gnome.navi.cx/"
depends=('libgnomeui>=2.18.1-2' 'libsexy>=0.1.11' 'gconf>=2.18.0.1-4'
         'hicolor-icon-theme' 'desktop-file-utils' 'libcanberra' 'libsm')
makedepends=('gettext' 'perlxml' 'python2' 'tcl' 'libnotify' 'gnome-doc-utils>=0.10.3'
             'intltool' 'pkg-config')
optdepends=('libnotify: notifications'
            'python2: plugin'
            'tcl: plugin')
options=('!libtool')
install=xchat-gnome.install
source=(http://ftp.gnome.org/pub/gnome/sources/${pkgname}/0.26/${pkgname}-${pkgver}.tar.gz
        gtk2-2.20.patch xchat-gnome-0.26.1-deprecated-symbol.patch)
md5sums=('e355d71d76cd97a0764e37bfacf09101' 'dafc2536a0c5ee3f8015af81fce69656'\
         'f82417277f0b5e83334147c4bf63d531')
sha1sums=('303bfbf2f1bda83fbc4edab148df4764cb4b8998' 'f1bfc5af50be7c10a9db96be20185325636d8da9'\
         '9aa0abc4b7999e09cafdeb8da1ad4641552a421b')

build() {
  cd "$srcdir/${pkgname}-${pkgver}"
  patch -Np1 -i "$srcdir/gtk2-2.20.patch"
  patch -Np1 -i "$srcdir/xchat-gnome-0.26.1-deprecated-symbol.patch"
  sed -i 's/notify_notification_new (summary, escaped, NULL, NULL);/notify_notification_new (summary, escaped, NULL);/' plugins/notify-osd/notify-osd.c
  sed -i -e "s/       /\t/" src/common/dbus/Makefile.in

  ./configure --prefix=/usr --sysconfdir=/etc \
              --localstatedir=/var --disable-static \
	      --enable-ipv6 --enable-shm \
	      --disable-gtkfe --disable-scrollkeeper
  make
}

package() {  
  cd "$srcdir/${pkgname}-${pkgver}"
  make GCONF_DISABLE_MAKEFILE_SCHEMA_INSTALL=1 DESTDIR="${pkgdir}" install
  
  install -d "$pkgdir/usr/share/gconf/schemas"
  gconf-merge-schema "$pkgdir/usr/share/gconf/schemas/${pkgname}.schemas" \
    "$pkgdir"/etc/gconf/schemas/*.schemas
  rm -f "$pkgdir"/etc/gconf/schemas/*.schemas
}
