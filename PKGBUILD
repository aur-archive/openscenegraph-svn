# Maintainer: A. Weiss <adam [at] archlinux.us>
# Contributor: Thomas Dziedzic < gostrc at gmail >
# Contributor: Hans Janssen <janserv@gmail.com>
# Contributor: my64 <packages@obordes.com>
# Contributor: Colin Pitrat <colin.pitrat@gmail.com>
# Contributor: smradlev

####################################################
# Set the following to true or false
####################################################
_buildexamples=true
_downloadsampledata=true
####################################################

pkgname=openscenegraph-svn
pkgver=12151
pkgrel=1
pkgdesc='An open source, high performance real-time graphics toolkit.'
arch=('i686' 'x86_64')
license=('custom:OSGPL')
url=('http://www.openscenegraph.org')
depends=('giflib' 'jasper' 'librsvg' 'xine-lib' 'curl')
makedepends=('cmake' 'svn')
optdepends=('libvncserver: VNC client widget'
            'qt: OSG/Qt integration through osgQt'
            'openexr: OpenEXR plugin'
            'openal: OpenAL plugin'
            'wxwidgets: wxWidgets integration'
            'gdal: Geospatial data support')
conflicts=('openthreads' 'openscenegraph')
provides=('openthreads' 'openscenegraph')
install='osg.install'
source=('LICENSE')
sha512sums=('7d3c7d5c920a2ca3958916685c79b8b829dc5d351a65dfc2a359b114d92f5f91644c562275dbb0b951df745432452eac146286ef7bf0fe128bb94ca743bb90ea')

_svntrunk="http://svn.openscenegraph.org/osg/OpenSceneGraph/trunk"
_svnmod="trunk"

build() {

  cd $srcdir

  if [[ -d $_svnmod ]]; then
    svn up -r $pkgver $_svnmod
  else
    svn co $_svntrunk --config-dir ./ -r $pkgver $_svnmod
  fi

  msg "SVN checkout done or server timeout"
  msg "Starting make..."

  rm -rf build
  mkdir -p build
  cd build

  cmake ../$_svnmod \
    -DBUILD_OSG_EXAMPLES=$_buildexamples \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DCMAKE_BUILD_TYPE=Release

  make

  if [[ $_downloadsampledata ]] ; then
    cd $srcdir
    rm -rf OpenSceneGraph-Data
    svn co http://svn.openscenegraph.org/osg/OpenSceneGraph-Data/trunk OpenSceneGraph-Data
  fi
}

package() {
  cd build
  make DESTDIR=$pkgdir install
  cp -r $srcdir/OpenSceneGraph-Data $pkgdir/usr/share/
  mkdir -p $pkgdir/usr/share/licenses/$pkgname
  cp $srcdir/LICENSE $pkgdir/usr/share/licenses/$pkgname
}
