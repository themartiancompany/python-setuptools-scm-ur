# Maintainer: Felix Yan <felixonmars@archlinux.org>
# Contributor: Hugo Osvaldo Barrera <hugo@barrera.io>

pkgbase=python-setuptools-scm
pkgname=(python-setuptools-scm python2-setuptools-scm)
pkgver=1.11.0
pkgrel=1
pkgdesc="Handles managing your python package versions in scm metadata."
arch=('any')
url="https://github.com/pypa/setuptools_scm"
license=('MIT')
makedepends=('python-setuptools' 'python2-setuptools' 'git')
checkdepends=('python-pytest-runner' 'python2-pytest-runner' 'mercurial')
source=("git+https://github.com/pypa/setuptools_scm.git#commit=$_commit")
md5sums=('SKIP')

prepare() {
  cp -a setuptools_scm{,-py2}
}

build() {
  cd "$srcdir"/setuptools_scm
  python setup.py build
  python setup.py egg_info

  cd "$srcdir"/setuptools_scm-py2
  python2 setup.py build
  python2 setup.py egg_info
}

check() {
  # Hack entry points by installing it

  cd "$srcdir"/setuptools_scm
  python setup.py install --root="$PWD/tmp_install" --optimize=1
  PYTHONPATH="$PWD/tmp_install/usr/lib/python3.5/site-packages:$PYTHONPATH" python setup.py ptr

  cd "$srcdir"/setuptools_scm-py2
  python2 setup.py install --root="$PWD/tmp_install" --optimize=1
  PYTHONPATH="$PWD/tmp_install/usr/lib/python2.7/site-packages:$PYTHONPATH" python2 setup.py ptr
}

package_python-setuptools-scm() {
  depends=('python-setuptools')
  provides=('python-setuptools_scm')
  conflicts=('python-setuptools_scm')
  replaces=('python-setuptools_scm')

  cd "$srcdir"/setuptools_scm
  python setup.py install --root "$pkgdir"

  install -Dm644 LICENSE "$pkgdir"/usr/share/licenses/$pkgname/LICENSE
}

package_python2-setuptools-scm() {
  depends=('python2-setuptools')
  provides=('python2-setuptools_scm')
  conflicts=('python2-setuptools_scm')
  replaces=('python2-setuptools_scm')

  cd "$srcdir"/setuptools_scm-py2
  python2 setup.py install --root "$pkgdir"

  install -Dm644 LICENSE "$pkgdir"/usr/share/licenses/$pkgname/LICENSE
}
