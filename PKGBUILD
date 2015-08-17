# Maintainer: Niklas Wallén <nikw at gmx dot com>
# Contributor: M Rawash <mrawash@gmail.com>

pkgname=textadept-beta
_version=8.0
pkgver=8.0
pkgrel=1
pkgdesc="A fast, minimalist, and remarkably extensible cross-platform text editor"
arch=('i686' 'x86_64')
url="http://foicica.com/textadept"
license=('MIT')
depends=('gtk2' 'desktop-file-utils')
optdepends=(
  'textadept-modules-beta: for official modules'
  'ncurses: for the terminal version of Textadept'
  'discount: for generating docs for your own modules'
  'lua51-doc: for generating docs for your own modules'
)
makedepends=('wget' 'unzip')
provides=('textadept')
conflicts=('textadept')
install='textadept.install'
source=("http://foicica.com/textadept/download/textadept_${_version}.i386.tgz"
        'textadept.install')
md5sums=('4e116560c487ce0b70033869f0531b22'
         'b94be5feb9378a2e55b1295ddae8a49b')

prepare() {
  cd "$srcdir/textadept_${_version}.i386/src"
  make -j1 deps
  cp lua.sym luajit/src/
}

build() {
  cd "$srcdir/textadept_${_version}.i386/src"
  make -j1
  make -j1 curses

  # don't run this - the docs are already fine, this messes it up
#  make -j1 doc
}

package() {
  cd "$srcdir/textadept_${_version}.i386/src"
  make -j 1 DESTDIR="$pkgdir" PREFIX=/usr install
  make -j 1 DESTDIR="$pkgdir" PREFIX=/usr curses install

  install -d "$pkgdir/opt"
  mv "$pkgdir/usr/share/textadept" "$pkgdir/opt/"

  # freedesktop.org desktop entry
  install -Dm644 "textadept.desktop" \
                 "$pkgdir/usr/share/applications/textadept.desktop"

  # links to binaries
  rm -rf "$pkgdir/usr/bin"
  install -d "$pkgdir/usr/bin"
  ln -s "/opt/textadept/textadept"{,jit}{,-curses} "$pkgdir/usr/bin/"

  # link to license
  install -d "$pkgdir/usr/share/licenses/$pkgname"
  ln -s "/opt/textadept/LICENSE" "$pkgdir/usr/share/licenses/$pkgname/"

  # link to documentation
  install -d "$pkgdir/usr/share/doc"
  ln -s /opt/textadept/doc "$pkgdir/usr/share/doc/textadept"

  # remove windows icons
  rm "$pkgdir"/opt/textadept/core/images/*.ico
  rm "$pkgdir"/opt/textadept/core/images/textadept.icns
  # links to icons
  for res in 16 32 48 64 128 256; do
    install -d "${pkgdir}/usr/share/icons/hicolor/${res}x${res}/apps"
    ln -s "/opt/textadept/core/images/ta_${res}x${res}.png" \
       "$pkgdir/usr/share/icons/hicolor/${res}x${res}/apps/textadept.png"
  done
  install -d "$pkgdir/usr/share/icons/hicolor/scalable/apps"
  ln -s "/opt/textadept/core/images/textadept.svg" \
     "$pkgdir/usr/share/icons/hicolor/scalable/apps/textadept.svg"

  # clean up
  #rm "$pkgdir/opt/textadept/doc/bombay"
  #rm "$pkgdir"/opt/textadept/lexers/.hg_archival.txt
  #rm "$pkgdir"/opt/textadept/core/.*.luadoc
}
