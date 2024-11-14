# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>
# Maintainer: Felix Yan <felixonmars@archlinux.org>
# Maintainer: Caleb Maclennan <caleb@alerque.com>

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
_proj="poetry"
_pkg="${_proj}-plugin-export"
pkgname="${_py}-${_pkg}"
pkgver=1.8.0
pkgrel=1
_pkgdesc=(
  "Poetry plugin to export"
  "the dependencies to various formats."
)
_http="https://github.com"
_ns="python-${_proj}"
url="${_http}/${_ns}/${_pkg}"
license=(
  'MIT'
)
arch=(
  'any'
)
depends=(
  "${_py}>=${_pymajver}"
  "${_py}<${_pynextver}"
  "${_py}-${_proj}"
)
makedepends=(
  "${_py}-build"
  "${_py}-installer"
  "${_py}-wheel"
)
checkdepends=(
  "${_py}-pytest-mock"
  "${_py}-pytest-xdist"
)
source=(
  "${url}/archive/${pkgver}/${pkgname}-${pkgver}.tar.gz"
)
sha512sums=(
  'ddaa4ed20601357648fe0187c50d033da5dfc37d66c96b67c83962a88825ec5b49c79b30e60f67c860d381e6ad0cbac084209af806561f50cdc779dcebacaf2b'
)

build() {
  cd \
    "${pkgname}-${pkgver}"
  "${_py}" \
    -m \
      build \
    -wn
}

check() {
  local \
    site_packages
  site_packages=$( \
    "${_py}" \
      -c \
        "import site; print(site.getsitepackages()[0])")
  cd \
    "${pkgname}-${pkgver}"
  "${_py}" \
    -m \
      installer \
    --destdir \
      tmp_install \
    dist/*.whl
  PYTHONPATH="$PWD/tmp_install/$site_packages" \
  pytest
}

package() {
  cd \
    "${pkgname}-${pkgver}"
  "${_py}" \
    -m \
      installer \
    --destdir="${pkgdir}" \
    dist/*.whl
  install \
    -Dm644 \
    -t \
    "${pkgdir}/usr/share/licenses/${pkgname}/" \
    LICENSE
}
