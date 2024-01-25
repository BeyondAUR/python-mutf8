# Maintainer: 0x9fff00 <0x9fff00+git@protonmail.ch>
# Contributor: Luis Martinez <luis dot martinez at disroot dot org>

_name=mutf8
pkgname=python-$_name
pkgver=1.0.6
pkgrel=3
pkgdesc='MUTF-8 encoder/decoder'
arch=('x86_64')
url="https://github.com/TkTech/mutf8"
license=('MIT')
depends=('python')
makedepends=('python-build' 'python-installer' 'python-setuptools' 'python-wheel')
checkdepends=('python-pytest')
# tests directory isn't in pypi sdist
source=("$pkgname-$pkgver.tar.gz::https://github.com/TkTech/mutf8/releases/tag/v${pkgver}")
sha256sums=('fe3a15bd76f26abf96036233a1a10974a977055db5dd14e6e5cd8ba76157307f')

build() {
  cd "${srcdir}/${_name}-${pkgver}"

  python -m build --wheel --no-isolation
}

check() {
  cd "${srcdir}/${_name}-${pkgver}"

  # https://wiki.archlinux.org/title/Python_package_guidelines#Check
  local _python_version="$(python -c 'import sys; print("".join(map(str, sys.version_info[:2])))')"
  PYTHONPATH="$PWD/build/lib.linux-$CARCH-cpython-$_python_version" pytest -x
}

package() {
  cd "${srcdir}/${_name}-${pkgver}"

  python -m installer --destdir="$pkgdir/" dist/*.whl

  # https://wiki.archlinux.org/title/Python_package_guidelines#Using_site-packages
  local _site_packages="$(python -c 'import site; print(site.getsitepackages()[0])')"

  install -d "$pkgdir/usr/share/licenses/$pkgname"
  local _license_path="$_site_packages/$_name-$pkgver.dist-info/LICENCE"
  [ -f "$pkgdir/$_license_path" ] || { echo "License file not found"; exit 1; }
  ln -s "$_license_path" "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
