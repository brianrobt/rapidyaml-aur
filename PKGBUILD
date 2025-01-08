# Maintainer: Maxime Gauduin <alucryd@archlinux.org>
# Contributor: xiretza <xiretza+aur@xiretza.xyz>

pkgbase=rapidyaml
pkgname=(
  rapidyaml
  python-rapidyaml
)
pkgver=0.7.2
pkgrel=1
pkgdesc='A library to parse and emit YAML, and do it fast'
arch=(x86_64)
url='https://github.com/biojppm/rapidyaml'
license=(MIT)
makedepends=(
  cmake
  git
  ninja
  python-setuptools
  python-setuptools-git
  python-setuptools-scm
  python-cmake-build-extension
  swig
)
_tag=9b8df62d9a45c050804305462b243add936c9292
source=(
  git+https://github.com/biojppm/rapidyaml.git#tag=${_tag}
  git+https://github.com/biojppm/c4core.git
  c4core-cmake::git+https://github.com/biojppm/cmake.git
)
b2sums=('SKIP'
        'SKIP'
        'SKIP')

pkgver() {
  cd rapidyaml
  git describe --tags | sed 's/^v//'
}

prepare() {
  cd rapidyaml
  git submodule init
  git config submodule.extern/c4core.url "${srcdir}"/c4core
  git -c protocol.file.allow=always submodule update
  cd ext/c4core
  git submodule init
  git config submodule.cmake.url "${srcdir}"/c4core-cmake
  git -c protocol.file.allow=always  submodule update
}

build() {
  cmake -B build -S rapidyaml -G Ninja \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DBUILD_SHARED_LIBS=ON \
    -DRYML_BUILD_API=ON \
    -DRYML_DEFAULT_CALLBACK_USES_EXCEPTIONS=ON
  cmake --build build
  cd rapidyaml
  python setup.py build
}

package_rapidyaml() {
  depends=(
    gcc-libs
    glibc
  )
  provides=(
    libc4core.so
    libryml.so
  )

  DESTDIR="${pkgdir}" cmake --install build
  rm "${pkgdir}"/usr/{_ryml.so,ryml.py}
  install -Dm 644 rapidyaml/LICENSE.txt -t "${pkgdir}"/usr/share/licenses/rapidyaml/
}

package_python-rapidyaml() {
  depends=(
    gcc-libs
    glibc
    python
  )

  cd rapidyaml
  python setup.py install --root="${pkgdir}" --optimize=1 --skip-build
  install -Dm644 LICENSE.txt -t "${pkgdir}"/usr/share/licenses/python-rapidyaml
}

# vim: ts=2 sw=2 et:
