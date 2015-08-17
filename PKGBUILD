# $Id: PKGBUILD 193533 2013-08-22 14:50:14Z andrea $
# Maintainer: Andrea Scarpino <andrea@archlinux.org>
# Contributor: Douglas Soares de Andrade <douglas@archlinux.org>
# Contributor: riai <riai@bigfoot.com> Ben <ben@benmazer.net>

_snapshot=bfe6e40923a4
pkgname=pyqt4dev
true && pkgname=('pyqt4-common' 'python-pyqt4' 'python2-pyqt4')
pkgver=4.10.4dev
pkgrel=10
arch=('i686' 'x86_64')
url='http://www.riverbankcomputing.com/software/pyqt/intro'
license=('GPL')
makedepends=('python-sip' 'python-dbus' 'sip>=4.15.5dev' 'phonon' 'mesa'
             'python2-opengl' 'qt-assistant-compat' 'qtwebkit' 'python2-dbus')
source=("http://www.riverbankcomputing.com/static/Downloads/PyQt4/PyQt-x11-gpl-4.10.4-snapshot-$_snapshot.tar.gz")
md5sums=('83c6175f8e5c81ea8f9420a649a97e90')


provides=('pyqt4')
conflicts=('pyqt4')


build() {
  cp -r PyQt-x11-gpl-4.10.4-snapshot-$_snapshot Py2Qt-x11-gpl-4.10.4-snapshot-$_snapshot

  cd PyQt-x11-gpl-4.10.4-snapshot-$_snapshot
  python configure.py \
    --confirm-license \
    --qsci-api \
    -q /usr/bin/qmake-qt4

  # Thanks Gerardo for the rpath fix
  find -name 'Makefile' | xargs sed -i 's|-Wl,-rpath,/usr/lib||g;s|-Wl,-rpath,.* ||g'

  make

  ### Python2 version ###
  cd ../Py2Qt-x11-gpl-4.10.4-snapshot-$_snapshot
  python2 configure.py \
    --confirm-license \
    -v /usr/share/sip \
    --qsci-api \
    -q /usr/bin/qmake-qt4

  # Thanks Gerardo for the rpath fix
  find -name 'Makefile' | xargs sed -i 's|-Wl,-rpath,/usr/lib||g;s|-Wl,-rpath,.* ||g'

  make
}

package_pyqt4-common(){
  pkgdesc="Common PyQt files shared between python-pyqt4 and python2-pyqt4"
  depends=('qt4')
  replaces=('pyqt-common')
  conflicts=('pyqt-common')
  provides=("pyqt-common=${pkgver}")

  cd PyQt-x11-gpl-4.10.4-snapshot-$_snapshot
  make -C pyrcc DESTDIR="${pkgdir}" install
  make -C pylupdate DESTDIR="${pkgdir}" install

  install -Dm644 PyQt4.api "${pkgdir}"/usr/share/qt4/qsci/api/python/PyQt4.api
}

package_python-pyqt4(){
  pkgdesc="A set of Python 3.x bindings for the Qt toolkit"
  depends=('qtwebkit' 'python-sip' 'python-dbus' 'pyqt4-common')
  optdepends=('phonon: enable audio and video in PyQt applications'
              'qscintilla: QScintilla API'
              'qt-assistant-compat: add PyQt online help in Qt Assistant')
  replaces=('pyqt')
  conflicts=('pyqt')
  provides=("pyqt=${pkgver}")

  cd PyQt-x11-gpl-4.10.4-snapshot-$_snapshot
  # INSTALL_ROOT is needed for the QtDesigner module, the other Makefiles use DESTDIR
  make DESTDIR="${pkgdir}" INSTALL_ROOT="${pkgdir}" install

  # Provided by pyqt-common
  rm "${pkgdir}"/usr/bin/{pylupdate4,pyrcc4}
  rm "${pkgdir}"/usr/share/qt4/qsci/api/python/PyQt4.api
}

package_python2-pyqt4(){
  pkgdesc="A set of Python 2.x bindings for the Qt toolkit"
  depends=('qtwebkit' 'python2-sip' 'python2-dbus' 'pyqt4-common')
  optdepends=('phonon: enable audio and video in PyQt applications'
              'python2-opengl: enable OpenGL 3D graphics in PyQt applications'
              'qscintilla: QScintilla API'
              'qt-assistant-compat: add PyQt online help in Qt Assistant')
  replaces=('python2-pyqt')
  conflicts=('python2-pyqt')
  provides=("python2-pyqt=${pkgver}")

  cd Py2Qt-x11-gpl-4.10.4-snapshot-$_snapshot
  # INSTALL_ROOT is needed for the QtDesigner module, the other Makefiles use DESTDIR
  make DESTDIR="${pkgdir}" INSTALL_ROOT="${pkgdir}" install

  # Fix conflicts with python-pyqt4
  mv "${pkgdir}"/usr/bin/{,python2-}pyuic4

  # Provided by python-pyqt4
  rm "${pkgdir}"/usr/bin/{pylupdate4,pyrcc4}
  rm "${pkgdir}"/usr/lib/qt4/plugins/designer/libpyqt4.so
  rm "${pkgdir}"/usr/share/qt4/qsci/api/python/PyQt4.api
}
