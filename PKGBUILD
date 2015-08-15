# Maintainer: Thijs Vermeir <thijsvermeir@gmail.com>

_realname=gstreamer0.10
_upstreamver=0.10.36
pkgname=${_realname}-git
pkgver=20130218
pkgrel=1
pkgdesc="GStreamer Multimedia Framework"
arch=(i686 x86_64)
license=('LGPL')
url="http://gstreamer.freedesktop.org/"
depends=('libxml2' 'glib2')
optdepends=('sh: feedback script')
conflicts=("${_realname}")
makedepends=('git' 'intltool' 'pkgconfig' 'gtk-doc' 'gobject-introspection')
provides=("${_realname}=${_upstreamver}")
options=('!libtool')
groups=('gstreamer')

_gitroot=git://anongit.freedesktop.org/gstreamer/gstreamer
_gitname=master

build() {
    cd ${srcdir}
    if [ -d gstreamer ] ; then
      cd gstreamer && git pull && cd ..
    else
      git clone git://anongit.freedesktop.org/gstreamer/gstreamer
      cd gstreamer
      git checkout 0.10
      cd ..
    fi

  rm -rf build
  cp -r gstreamer build
  cd build

  sed 's|AM_CONFIG_HEADER|AC_CONFIG_HEADERS|' -i configure.ac
  ./autogen.sh --noconfigure
  ./configure --prefix=/usr --sysconfdir=/etc --localstatedir=/var --libexecdir=/usr/lib \
    --with-package-name="GStreamer (Archlinux)" \
    --with-package-origin="http://www.archlinux.org/" \
    --disable-gtk-doc --disable-static

    make
}

check() {
  cd "${srcdir}/build"
  make check
}

package() {
  cd "${srcdir}/build"
  make DESTDIR="${pkgdir}" install

  #Remove unversioned gst-* binaries to get rid of conflicts
  cd "${pkgdir}/usr/bin"
  for bins in `ls *-0.10`; do
    rm -f ${bins/-0.10/}
  done
}
