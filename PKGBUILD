# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Pellegrino Prevete <pellegrinoprevete@gmail.com>
# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Felix Yan <felixonmars@archlinux.org>
# Maintainer: Daniel M. Capella <polyzen@archlinux.org>
# Contributor: Hugo Osvaldo Barrera <hugo@barrera.io>

_git=false
_pkg=setuptools-scm
# _Pkg=setuptools-scm
_py="python"
_pyver="$( \
  "${_py}" \
    -V | \
    awk \
      '{print $2}')"
_pymajver="${_pyver%.*}"
_pyminver="${_pymajver#*.}"
_pynextver="${_pymajver%.*}.$(( \
  ${_pyminver} + 1))"
pkgname="${_py}-${_pkg}"
pkgver=8.1.0
pkgrel=1
pkgdesc="Handles managing your python package versions in scm metadata"
arch=(
  'any'
)
_http="https://github.com"
_ns='pypa'
url="${_http}/${_ns}/${_pkg}"
license=(
  'MIT'
)
depends=(
  "${_py}>=${_pymajver}"
  "${_py}<${_pynextver}"
  "${_py}-packaging"
  "${_py}-setuptools"
  "${_py}-typing-extensions"
)
makedepends=(
  "${_py}-build"
  "${_py}-installer"
  "${_py}-wheel"
  "${_py}-pyproject-hooks>=1.2.1"
)
[[ "${_git}" == true ]] && \
  makedepends+=(
    'git'
  )

checkdepends=(
  'mercurial'
  "${_py}-pip"
  "${_py}-pytest"
  "${_py}-rich"
)
if [[ "${_git}" == "true" ]]; then
  _source="${_pkg}-${pkgver}::git+${url}#tag=${pkgver}?signed"
  _sum='SKIP'
else
  _source="${_pkg}-${pkgver}.tar.gz::${url}/archive/refs/tags/v${pkgver}.tar.gz"
  _sum="ec9b751aa65db429a22bfae64a66e3419afe092c88801104f04e4dfdc76c9de255262006b33d4f4af29790b7e0dc5f335b1e551bf35f5adf4fb3feeafca5bd68"
fi
source=(
  "${_source}"
)
b2sums=(
  "${_sum}"
)

build() {
  cd \
    "${_pkg}-${pkgver}"
  "${_py}" \
    -m \
      build \
    --wheel \
    --skip-dependency-check \
    --no-isolation
}

check() {
  cd \
    "${_pkg}-${pkgver}"
  "${_py}" \
    -m \
      venv \
    --system-site-packages \
    test-env
  test-env/bin/python \
    -m \
      installer \
    dist/*.whl
  test-env/bin/python \
    -m \
      pytest \
    -v \
    -k \
      'not test_not_owner'
}

package() {
  local \
    site_packages
  cd \
    "${_pkg}-${pkgver}"
  "${_py}" \
    -m \
      installer \
    --destdir="${pkgdir}" \
    dist/*.whl
  # Symlink license file
  site_packages="$( \
    "${_py}" \
      -c \
        "import site; print(site.getsitepackages()[0])")"
  install \
    -d \
    "${pkgdir}/usr/share/licenses/${pkgname}"
  ln \
    -s \
    "${site_packages}/${_pkg}-${pkgver}.dist-info/LICENSE" \
    "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}

