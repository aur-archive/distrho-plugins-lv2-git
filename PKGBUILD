# Contributor: Jiri Prochazka <ojirio@gmail.com> 

pkgname=distrho-plugins-lv2-git
pkgver=20120324
pkgrel=1
pkgdesc="A collection of lv2 plugins ported from VST versions, including TAL plugins (Noisemaker)"
arch=('i686' 'x86_64')
url="http://distrho.sourceforge.net/"
depends=('gcc-libs' 'fftw' 'libxext' 'freetype2')
makedepends=('premake3')
license=('custom')
#options=(!libtool !strip)

_gitroot="git://distrho.git.sf.net/gitroot/distrho/distrho"
_gitname="distrho"

build() {


  cd "$srcdir"
  msg "Connecting to GIT server...."

  if [ -d $_gitname ] ; then
    cd $_gitname && git pull origin
    msg "The local files are updated."
  else
    git clone $_gitroot $_gitname
  fi

  msg "GIT checkout done or server timeout"
  msg "Starting make..."

  rm -rf "$srcdir/$_gitname-build"
  git clone "$srcdir/$_gitname" "$srcdir/$_gitname-build"
  cd "$srcdir/$_gitname-build"

  #
  # BUILD HERE
  #

  # generate build script
  scripts/premake-update.sh linux

  # --as-needed incompatible
  export LDFLAGS="${LDFLAGS//,--as-needed}"

  make lv2
}

package() {
  cd "$srcdir/$_gitname-build"

  # bins
  install -d "$pkgdir/usr/bin"
  install -Dm755 bin/* \
    "$pkgdir/usr/bin"

  # HybridReverb2 data
  cd ports/hybridreverb2/data
  install -Dm644 HybridReverb2.conf \
    "$pkgdir/etc/HybridReverb2/HybridReverb2.conf"
  install -d "$pkgdir/usr/share"
  cp -a HybridReverb2 \
    "$pkgdir/usr/share"
}

