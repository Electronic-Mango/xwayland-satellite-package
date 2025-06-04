# Maintainer: David Runge <dvzrv@archlinux.org>

pkgname=xwayland-satellite
pkgver=0.6
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
sha512sums=('8b03cc50b883fa9417f92430be72abdc57bd9ed3b973e51fe8b0ad01485470c69f8284dda325447ae670624ef32f1167aff5b5c57681990d0e55c6178ac226c6')
b2sums=('905471a512c49e0c07154d5ccaecf5f37f4e282e2bbfbb3bbde79f198151ac9c7fdb888b8b8a502a8c9bc49cbebba465a5f9e12b97af054b0d0aef30d3a44885')

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
