# Maintainer: David Runge <dvzrv@archlinux.org>

pkgname=xwayland-satellite
pkgver=0.5
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
sha512sums=('89d57241d69f7f35b8c68019cedbe07f9c43601db27fcfacf9725391a3969a30fa0d1f21ae50762e0c64c674a80a6859d529130881056673499363f4dfbc258b')
b2sums=('b2ab19629c2b9d099af2c2dfa81a215acb1bc0ddc6a98574a05791cdcfeb980301c33657e4f5bbc4b64c30bc08e49e8f5fcb5fd82f8103d7dc1d25f10a20b5dc')

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
