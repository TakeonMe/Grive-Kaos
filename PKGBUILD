pkgname=Grive-KaoS
pkgver=0
pkgrel=1
pkgdesc="Open source Linux client for Google Drive"
arch=('x86_64')
url="http://www.lbreda.com/grive/start"
license=('GPL')
categories=('network')

depends=('json-c' 'curl' 'boost-libs' 'expat' 'libgcrypt' 'yajl')

makedepends=('cmake' 'git' 'boost' 'yajl' 'json-c')
options=(!emptydirs)

source=("${pkgname}-v${pkgver}-${pkgrel}-src.tar.gz::https://nodeload.github.com/Grive/grive/tar.gz/master")
md5sums=('51ba8b5fcf1f7c1297c8ca941723b9a7')

build() {
cd "$srcdir/grive-master"
sed -i -e '/find_package(BFD)/d' libgrive/CMakeLists.txt 
sed -i -e '43d' bgrive/CMakeLists.txt 

if [ "${CARCH}" = 'i686' ]; then
sed -i '251s|m_last_sync.Sec() ) )|(boost::uint64_t)m_last_sync.Sec() ) )|g' libgrive/src/drive/State.cc
sed -i '252s|m_last_sync.NanoSec() ) )|(boost::uint64_t)m_last_sync.NanoSec() ) )|g' libgrive/src/drive/State.cc
sed -i '256s|m_cstamp) |(boost::uint64_t)m_cstamp) |g' libgrive/src/drive/State.cc
fi 

rm -rf build
mkdir build
cd build
cmake .. -DCMAKE_BUILD_TYPE=Release \
 -DCMAKE_INSTALL_PREFIX=/usr -DCMAKE_EXE_LINKER_FLAGS=-ljson-c
sed -i '225s|libjson|libjson-c|g' CMakeCache.txt
make 
}
 
package() {
cd "$srcdir/grive-master/build"
make DESTDIR="$pkgdir" install 
}
