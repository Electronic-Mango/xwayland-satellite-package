# Maintainer: David Runge <dvzrv@archlinux.org>

pkgname=xwayland-satellite
pkgver=0.8
pkgrel=1
pkgdesc="Xwayland outside your Wayland"
arch=(x86_64)
url="https://github.com/Supreeeme/xwayland-satellite"
license=(MPL-2.0)
depends=(
  gcc-libs
  glibc
  libxcb
  xcb-util-cursor
  xorg-xwayland
)
makedepends=(
  clang
  rust
)
source=("$pkgname-$pkgver.tar.gz::$url/archive/refs/tags/v$pkgver.tar.gz")
sha512sums=('f53121f7367e1b7326b8910e58ed8d3086fa1a3d18e9cf4f6d52041ca1c1e621e44ecf6d357edefdb93f7df56c067e999f8da52e9b39ebf2b94965e17af77b53')
b2sums=('d4a920127f8a27a830bcdc768a738cd6d1fb5fadabded7416b53b237ddc3d44437b46675664e16c671cd21ec9222317273069b6474ccb1e9e50b277ac6432672')

prepare() {
  cd $pkgname-$pkgver
  sed 's|/usr/local|/usr|' -i resources/$pkgname.service
  export RUSTUP_TOOLCHAIN=stable
  cargo fetch --locked --target "$(rustc -vV | sed -n 's/host: //p')"
}

build() {
  cd $pkgname-$pkgver
  export RUSTUP_TOOLCHAIN=stable
  export CARGO_TARGET_DIR=target
  cargo build --frozen --release --features systemd
}

check() {
  cd $pkgname-$pkgver
  export XDG_RUNTIME_DIR="$(mktemp -d)"
  export RUSTUP_TOOLCHAIN=stable
  cargo test --frozen
}

package() {
  cd $pkgname-$pkgver
  install -vDm 755 target/release/$pkgname -t "$pkgdir/usr/bin/"
  install -vDm 644 resources/$pkgname.service -t "$pkgdir/usr/lib/systemd/user/"
  install -vDm 644 LICENSE -t "$pkgdir/usr/share/licenses/$pkgdir/"
}
