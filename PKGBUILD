# Maintainer: Jan Alexander Steffens (heftig) <heftig@archlinux.org>
# Contributor: Sergej Pupykin <pupykin.s+arch@gmail.com>
# Contributor: Alexander Fehr <pizzapunk gmail com>

pkgname=tracker3
pkgver=3.1.0
pkgrel=1
pkgdesc="Desktop-neutral user information store, search tool and indexer"
url="https://wiki.gnome.org/Projects/Tracker"
arch=(x86_64)
license=(GPL)
depends=(sqlite icu glib2 libffi util-linux libstemmer libseccomp libsoup
         json-glib)
makedepends=(gobject-introspection vala git gtk-doc bash-completion meson
             asciidoc systemd)
checkdepends=(python-gobject python-dbus)
provides=(libtracker-sparql-3.0.so)
groups=(gnome)
_commit=29219e06b9433cf188bdc56c9710cd466ddb3f75  # tags/3.1.0^0
source=("git+https://gitlab.gnome.org/GNOME/tracker.git#commit=$_commit")
sha256sums=('SKIP')

pkgver() {
  cd tracker
  git describe --tags | sed 's/_/./g;s/-/+/g'
}

prepare() {
  cd tracker
}

build() {
  arch-meson tracker build --buildtype debug
  meson compile -C build
}

check() {
  dbus-run-session meson test -C build --print-errorlogs -t 3
}

package() {
  DESTDIR="$pkgdir" meson install -C build
}
