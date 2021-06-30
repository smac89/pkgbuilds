# Maintainer: Christoph Schmidpeter <john.doe@example.com>
# Co-Maintainer: Mubashshir <ahmubashshir@gmail.com>
# Contributor: Iwan Timmer <irtimmer@gmail.com>
# pkg: git

pkgname=anbox-git
_pkgname=anbox
pkgver=r1350.04ac697
pkgrel=1
epoch=1
arch=('x86_64')
url="http://anbox.io/"
license=('GPL3')
pkgdesc="Running Android in a container"
depends=('lxc-git' 'sdl2_image' 'protobuf' 'anbox-image' 'libsystemd' 'boost-libs')
conflicts=('anbox-launchers-git')
makedepends=('cmake' 'git' 'glm' 'lxc' 'sdl2_image' 'protobuf' 'boost' 'properties-cpp' 'gtest' 'python')
source=("git+https://github.com/anbox/anbox.git"
	"git+https://github.com/google/cpu_features.git"
	"git+https://github.com/Kistler-Group/sdbus-cpp.git"
	'anbox-desktop-dir.patch'
	'lxc.patch'
	'anbox-container-manager.service'
	'anbox-session-manager.service'
	'99-anbox.rules'
	'anbox.conf'
	'anbox.desktop'
	'anbox-bridge.network'
	'anbox-bridge.netdev')
sha256sums=('SKIP'
            'SKIP'
            'SKIP'
            'd1e2e3fa1299b423f1d98cbd3c591d853f1ce8646f508ef838fe714f986fc5b2'
            '9212f8b941d62b50b9188aea6218ceb6f6b77761a388f9868b1071557a95e067'
            '5be94b63dc30d141f15ca7d1be6e3e81f26ef33f844614975537562f5d08236c'
            '1f22dbb5a3ca6925bbf62899cd0f0bbaa0b77c879adcdd12ff9d43adfa61b1d8'
            '210eb93342228168f7bb632c8b93d9bfda6f53f62459a6b74987fa1e17530475'
            '3e07dc524a827c1651857cce28a06c1565bc5188101c140ed213bbafedc5abff'
            '7332d09865be553a259a53819cebddd21f661c7a251d78c2f46acd75c66676b6'
            '44899328725667041e6e84912da81c1d0147b708006eb2c2bb6503f271629ff0'
            '559190df4d6d595480b30d8b13b862081fc4aac52790e33eb24cf7fbcb8003b8')

pkgver() {
  cd "$srcdir/$_pkgname"
  ( set -o pipefail
    git describe --long 2>/dev/null | sed 's/\([^-]*-g\)/r\1/;s/-/./g' ||
    printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
  )
}

prepare() {
  cd "$srcdir/${_pkgname}"

  # Don't build tests
  truncate -s 0 cmake/FindGMock.cmake
  truncate -s 0 tests/CMakeLists.txt
  sed -i '1i\#include <cstdint>' "$srcdir/anbox/src/anbox/input/manager.cpp"
  patch -p1 < "$srcdir/anbox-desktop-dir.patch"
  patch -p1 < "$srcdir/lxc.patch"

  git submodule init
  git config submodule.external/cpu_features.url $srcdir/cpu_features
  git config submodule.external/sdbus-cpp.url $srcdir/sdbus-cpp
  git submodule update
}

build() {
  mkdir -p "$srcdir/${_pkgname}/build"
  cd "$srcdir/${_pkgname}/build"

  cmake .. -DCMAKE_INSTALL_LIBDIR=/usr/lib -DCMAKE_INSTALL_PREFIX=/usr -DCMAKE_CXX_FLAGS="-Wno-error=implicit-fallthrough -Wno-error=missing-field-initializers" -DCMAKE_BUILD_TYPE=Release
  make
}

package() {
  cd "$srcdir/${_pkgname}"
  make -C build DESTDIR="$pkgdir" install

  install -Dm 644 -t $pkgdir/usr/lib/systemd/system $srcdir/anbox-container-manager.service
  install -Dm 644 -t $pkgdir/usr/lib/systemd/user $srcdir/anbox-session-manager.service
  install -Dm 644 $srcdir/anbox-bridge.network $pkgdir/usr/lib/systemd/network/80-anbox-bridge.network
  install -Dm 644 $srcdir/anbox-bridge.netdev $pkgdir/usr/lib/systemd/network/80-anbox-bridge.netdev
  install -Dm 644 -t $pkgdir/usr/lib/udev/rules.d $srcdir/99-anbox.rules
  install -Dm 644 -t $pkgdir/usr/share/applications $srcdir/anbox.desktop
  install -Dm 644 snap/gui/icon.png $pkgdir/usr/share/pixmaps/anbox.png
  install -Dm 644 -t $pkgdir/usr/share/desktop-directories     data/desktop/anbox-android.directory
  install -Dm 644 -t $pkgdir/etc/xdg/menus/applications-merged data/desktop/anbox-android.menu
}
