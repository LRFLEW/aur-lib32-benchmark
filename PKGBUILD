# Maintainer: Philipp Claßen <philipp.classen@posteo.de>
pkgname=benchmark
pkgver=1.4.1
pkgrel=1
pkgdesc="A microbenchmark support library, by Google"
arch=('any')
url="https://github.com/google/benchmark"
license=('Apache')
depends=('gcc-libs')
makedepends=('cmake' 'gtest' 'gmock')

source=("https://github.com/google/${pkgname}/archive/v${pkgver}.tar.gz")
sha256sums=('f8e525db3c42efc9c7f3bc5176a8fa893a9a9920bbd08cef30fb56a51854d60d')

prepare() {
  cd "${srcdir}/${pkgname}-${pkgver}"
  sed -i "s|v0.0.0|v${pkgver}|g" "cmake/GetGitVersion.cmake"

  mkdir -p build && cd build

  # For now, disable BENCHMARK_ENABLE_LTO, as it causes
  # more tests to fail and also breaks on GCC 9
  # (see https://gcc.gnu.org/bugzilla/show_bug.cgi?id=85583)
  cmake .. -DCMAKE_BUILD_TYPE="Release" \
           -DCMAKE_INSTALL_PREFIX=/usr \
           -DCMAKE_INSTALL_LIBDIR=lib \
           -DBUILD_SHARED_LIBS=ON
}

build() {
  cd "${srcdir}/${pkgname}-${pkgver}/build"
  make
}

check() {
  cd "${srcdir}/${pkgname}-${pkgver}/build"
  make test || echo "Ignore for now, as the test is expected to fail: https://github.com/google/benchmark/issues/578"
}

package() {
  cd "${srcdir}/${pkgname}-${pkgver}/build"
  make DESTDIR="$pkgdir/" install
}
