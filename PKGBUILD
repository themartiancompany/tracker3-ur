# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Jan Alexander Steffens (heftig) <heftig@archlinux.org>
# Maintainer: Fabian Bornschein <fabiscafe@archlinux.org>
# Contributor: Sergej Pupykin <pupykin.s+arch@gmail.com>
# Contributor: Alexander Fehr <pizzapunk gmail com>
# Contributor: Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>
# Contributor: Truocolo <truocolo@aol.com>

_py="python"
_docs=false
_pkg="tracker"
pkgbase="${_pkg}3"
pkgname=(
  "${pkgbase}"
)
[[ "${_docs}" == true ]] && \
  pkgname+=(
    "${pkgbase}-docs"
  )
pkgver=3.7alpha
pkgrel=1
pkgdesc="SQLite-based RDF triplestore database with SPARQL interface"
url="https://${_pkg}.gnome.org/"
arch=(
  x86_64
  arm
  aarch64
  powerpc
  pentium4
  i686
  armv7h
)
license=(
  LGPL-2.1-or-later
)
depends=(
  glib2
  icu
  json-glib
  libsoup3
  libstemmer
  libxml2
  sqlite
)
makedepends=(
  asciidoc
  bash-completion
  dbus
  git
  gobject-introspection
  libsoup
  meson
  python-dbus
  python-gobject
  python-tappy
  systemd
  vala
)
[[ "${_docs}" == true ]] & \
  makedepends+=(
    gi-docgen
  )
_commit=b310f3a9cbcb8c76ca08b3269cbe439485391e9e  # tags/3.7.alpha^0
source=(
  "git+https://gitlab.gnome.org/GNOME/${_pkg}.git#commit=$_commit"
  "git+https://gitlab.gnome.org/GNOME/gvdb.git"
)
b2sums=(
  'SKIP'
  'SKIP'
)

pkgver() {
  cd \
    "${_pkg}"
  git \
    describe \
      --tags | \
    sed \
      -r \
        's/\.([a-z])/\1/;s/([a-z])\./\1/;s/[^-]*-g/r&/;s/-/+/g'
}

prepare() {
  cd \
    "${_pkg}"
  git \
    submodule \
      init
  git \
    submodule \
      set-url \
        subprojects/gvdb \
        "$srcdir/gvdb"
  git \
    -c \
      protocol.file.allow=always \
    submodule \
      update
}

build() {
  local meson_options=()
  meson_options=(
    -D tests_tap_protocol=true
  )

  arch-meson \
    "${_pkg}" \
      build \
      "${meson_options[@]}"
  meson \
    compile \
      -C \
        build
}

check() {
  dbus-run-session \
    meson \
    test \
    -C \
      build \
      --print-errorlogs \
      -t \
        3
}

package_tracker3() {
  optdepends=(
    'libsoup: Alternative remoting backend'
  )
  provides=(
    "lib${_pkg}-sparql-3.0.so"
  )
  meson \
    install \
    -C build \
    --destdir "$pkgdir"
  if [[ "${_docs}" == true ]]; then
  mkdir \
    -p \
    docs/usr/share
  mv \
    {"$pkgdir",docs}"/usr/share/doc"
  fi
}

package_tracker3-docs() {
  pkgdesc+=" (documentation)"
  depends=()
  mv \
    docs/* \
    "$pkgdir"
}

# vim:set sw=2 sts=-1 et:
#!/usr/bin/env bash
#
# vim:set sw=2 sts=-1 et:
