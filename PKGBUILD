# Maintainer: Felix Yan <felixonmars@archlinux.org>
# Contributor: Hugo Osvaldo Barrera <hugo@barrera.io>

pkgbase=python-setuptools-scm
pkgname=(python-setuptools-scm python2-setuptools-scm)
pkgver=1.15.7
pkgrel=1
pkgdesc="Handles managing your python package versions in scm metadata."
arch=('any')
url="https://github.com/pypa/setuptools_scm"
license=('MIT')
makedepends=('python-setuptools' 'python2-setuptools')
checkdepends=('python-pytest' 'python2-pytest' 'mercurial' 'git' 'python-pip' 'python2-pip')
source=("$pkgbase-$pkgver.tar.gz::https://github.com/pypa/setuptools_scm/archive/v$pkgver.tar.gz")
sha512sums=('a231bbecef14c50606afbbccda7bb2a924e3ef2aa7c55bd7d41fbcce9687034fb339349d465bb9011c3c36dc16833126754fd8f2395e25c035b3cfee50787aea')

prepare() {
  cp -a setuptools_scm-$pkgver{,-py2}

  export SETUPTOOLS_SCM_PRETEND_VERSION=$pkgver
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
  SETUPTOOLS_SCM_PRETEND_VERSION= PYTHONPATH="$PWD/tmp_install/usr/lib/python3.6/site-packages:$PYTHONPATH" py.test

  cd "$srcdir"/setuptools_scm-$pkgver-py2
  python2 setup.py install --root="$PWD/tmp_install" --optimize=1
  SETUPTOOLS_SCM_PRETEND_VERSION= PYTHONPATH="$PWD/tmp_install/usr/lib/python2.7/site-packages:$PYTHONPATH" py.test2
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
