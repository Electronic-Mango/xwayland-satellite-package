# Maintainer: David Runge <dvzrv@archlinux.org>

pkgname=xwayland-satellite
pkgver=0.5.1
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
sha512sums=('31369add44d81607987a713968b80e8c237bbd545e734d1b856fe74a847623a714259c7229997bbf7a71147a3ca6a145f80ff17d7e551a33b5afe9cc4d422fb3')
b2sums=('1a5239b30edf5d66761c8a5654180c53a3761a385815116765419f9b4c2c28481fd0d4ec58405603434dac270a88fbd782a6359e930587acc77f5ba1abd6b5f3')

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
  cargo build --frozen --release --all-features
}

check() {
  cd $pkgname-$pkgver
  export XDG_RUNTIME_DIR="$(mktemp -d)"
  export RUSTUP_TOOLCHAIN=stable
  cargo test --frozen --all-features
}

package() {
  cd $pkgname-$pkgver
  install -vDm 755 target/release/$pkgname -t "$pkgdir/usr/bin/"
  install -vDm 644 resources/$pkgname.service -t "$pkgdir/usr/lib/systemd/user/"
  install -vDm 644 LICENSE -t "$pkgdir/usr/share/licenses/$pkgdir/"
}
