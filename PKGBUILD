# Maintainer: Felix Yan <felixonmars@archlinux.org>
# Contributor: Hugo Osvaldo Barrera <hugo@barrera.io>

pkgbase=python-setuptools-scm
pkgname=(python-setuptools-scm python2-setuptools-scm)
pkgver=3.0.5
pkgrel=1
pkgdesc="Handles managing your python package versions in scm metadata."
arch=('any')
url="https://github.com/pypa/setuptools_scm"
license=('MIT')
makedepends=('python-setuptools' 'python2-setuptools')
checkdepends=('python-pytest' 'python2-pytest' 'mercurial' 'git' 'python-pip' 'python2-pip')
source=("https://pypi.io/packages/source/s/setuptools_scm/setuptools_scm-$pkgver.tar.gz")
sha512sums=('536468949703b393349170d543b17ddc27b6c75a147d502e9570d166209ec6936273b6fa976f6e1eaacfa9ae9a507e156755f9f42cc3853c8e13fac398396810')

prepare() {
  cp -a setuptools_scm-$pkgver{,-py2}
}

build() {
  cd "$srcdir"/setuptools_scm-$pkgver
  python setup.py build
  python setup.py egg_info

  cd "$srcdir"/setuptools_scm-$pkgver-py2
  python2 setup.py build
  python2 setup.py egg_info
}

check() {
  # Hack entry points by installing it

  cd "$srcdir"/setuptools_scm-$pkgver
  python setup.py install --root="$PWD/tmp_install" --optimize=1
  PYTHONPATH="$PWD/tmp_install/usr/lib/python3.7/site-packages:$PYTHONPATH" py.test

  cd "$srcdir"/setuptools_scm-$pkgver-py2
  python2 setup.py install --root="$PWD/tmp_install" --optimize=1
  PYTHONPATH="$PWD/tmp_install/usr/lib/python2.7/site-packages:$PYTHONPATH" py.test2
}

package_python-setuptools-scm() {
  depends=('python-setuptools')
  provides=('python-setuptools_scm')
  conflicts=('python-setuptools_scm')
  replaces=('python-setuptools_scm')

  cd "$srcdir"/setuptools_scm-$pkgver
  python setup.py install --root "$pkgdir"

  install -Dm644 LICENSE "$pkgdir"/usr/share/licenses/$pkgname/LICENSE
}

package_python2-setuptools-scm() {
  depends=('python2-setuptools')
  provides=('python2-setuptools_scm')
  conflicts=('python2-setuptools_scm')
  replaces=('python2-setuptools_scm')

  cd "$srcdir"/setuptools_scm-$pkgver-py2
  python2 setup.py install --root "$pkgdir"

  install -Dm644 LICENSE "$pkgdir"/usr/share/licenses/$pkgname/LICENSE
}
