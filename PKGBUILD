# $Id$
# Maintainer: Jan Alexander Steffens (heftig) <jan.steffens@gmail.com>

pkgname=gnome-settings-daemon
pkgver=3.24.2
pkgrel=1
pkgdesc="GNOME Settings Daemon"
url="https://github.com/pfrybar/gnome-settings-daemon"
arch=(i686 x86_64)
license=(GPL)
depends=(dconf gnome-desktop gsettings-desktop-schemas libcanberra-pulse libnotify libsystemd
         libwacom pulseaudio pulseaudio-alsa upower librsvg libgweather geocode-glib geoclue2 nss
         libgudev gtk3-print-backends libnm)
makedepends=(intltool xf86-input-wacom libxslt docbook-xsl python git gnome-common)
groups=(gnome)
_commit=7c06b4a983b535bc3e20e40c3173434af68d79d1  # paul-gnome-3-24-2
source=("git+https://github.com/pfrybar/gnome-settings-daemon#commit=$_commit"
        "git+https://git.gnome.org/browse/libgnome-volume-control")
sha256sums=('SKIP'
            'SKIP')

pkgver() {
  cd $pkgname
  git describe --tags | sed 's/^GNOME_SETTINGS_DAEMON_//;s/_/./g;s/-/+/g'
}

prepare() {
  cd $pkgname

  git submodule init
  git config --local submodule."panels/media-keys/gvc".url "$srcdir/libgnome-volume-control"
  git submodule update

  NOCONFIGURE=1 ./autogen.sh
}

build() {
  cd $pkgname

  ./configure --prefix=/usr --sysconfdir=/etc --localstatedir=/var \
      --libexecdir=/usr/lib/$pkgname --disable-static

  #https://bugzilla.gnome.org/show_bug.cgi?id=656231
  sed -i -e 's/ -shared / -Wl,-O1,--as-needed\0/g' libtool

  make
}

package() {
  cd $pkgname
  make DESTDIR="$pkgdir" install
}
