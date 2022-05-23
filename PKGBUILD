# Maintainer: mark.blakeney at bullet-systems dot net
# Contributor: Tod Jackson <tod.jackson@gmail.com>
# Contributor: Özgür Sarıer <echo b3pndXJzYXJpZXIxMDExNjAxMTE1QGdtYWlsLmNvbQo= | base64 -d>
# Contributor: user6553591 <Message on Reddit>
# Contributor: P. Badredin <p dot badredin at gmail dot com>
# Contributor: Justin Blanchard <UncombedCoconut at gmail dot com>
# Contributor: Auguste Pop < auguste [at] gmail [dot] com >
# Contributor: niklas.fiekas at backscattering dot de

pkgname=stockfish
pkgver=15
pkgrel=2
epoch=1
pkgdesc="A strong chess engine written by Tord Romstad, Marco Costalba, Joona Kiiski"
arch=('x86_64' 'i686' 'armv7h' 'aarch64')
url="https://stockfishchess.org/"
license=('GPL3')
depends=('glibc')
source=("$pkgname-$pkgver.zip::https://github.com/official-stockfish/Stockfish/archive/sf_$pkgver.zip")
sha512sums=('96e0e5bc11d678c39b4b16286a6e1ec511fd75bcae704422fd65907930dda33bad28aa9b7d24d1d4092cc765dcc2a1fbe3d047916a26a0016e465788512c0913')

build() {
  cd "Stockfish-sf_${pkgver}/src"

  if [[ "$CARCH" == "armv7h" ]]; then
    _arch=armv7
  elif [[ "$CARCH" == "aarch64" ]]; then
    _arch=armv8
  elif [[ "$CARCH" == "i686" ]]; then
    _arch=x86-32
  elif grep -wq avx512dq /proc/cpuinfo && grep -wq avx512vl /proc/cpuinfo && grep -wq avx512_vnni /proc/cpuinfo; then
    _arch=x86-64-vnni512
  elif grep -wq avx512f /proc/cpuinfo && grep -wq avx512bw /proc/cpuinfo; then
    _arch=x86-64-avx512
  elif grep -wq bmi2 /proc/cpuinfo; then
    if grep -wq GenuineIntel /proc/cpuinfo; then
      _arch=x86-64-bmi2
    elif grep -wq AuthenticAMD /proc/cpuinfo && [[ "$(grep --max-count=1 'cpu family' /proc/cpuinfo | sed -e 's/^.*: //')" -ge 25 ]]; then
      _arch=x86-64-bmi2
    else
      # On AMD, bmi2 is emulated before Zen 3, so that using it is a slowdown
      _arch=x86-64-avx2
    fi
  elif grep -wq avx2 /proc/cpuinfo; then
    _arch=x86-64-avx2
  elif grep -wq sse4_1 /proc/cpuinfo && grep -wq popcnt /proc/cpuinfo; then
    _arch=x86-64-sse41-popcnt
  elif grep -wq ssse3 /proc/cpuinfo; then
    _arch=x86-64-ssse3
  elif grep -wq pni /proc/cpuinfo && grep -wq popcnt /proc/cpuinfo; then
    _arch=x86-64-sse3-popcnt
  else
    _arch=x86-64
  fi

  make profile-build ARCH="$_arch" COMP=gcc
}

package() {
  cd "Stockfish-sf_${pkgver}/src"
  make PREFIX="$pkgdir/usr" install
}

# vim:set ts=2 sw=2 et:
