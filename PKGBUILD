# Maintainer: David Runge <dvzrv@archlinux.org>

pkgname=xwayland-satellite
pkgver=0.8.2
pkgrel=1
pkgdesc="Xwayland outside your Wayland"
arch=(x86_64)
url="https://github.com/Supreeeme/xwayland-satellite"
license=(MPL-2.0)
depends=(
  glibc
  libgcc
  libxcb
  xcb-util-cursor
  xorg-xwayland
)
makedepends=(
  clang
  rust
)
source=("$pkgname-$pkgver.tar.gz::$url/archive/refs/tags/v$pkgver.tar.gz")
sha512sums=('a125fe92591c7c39c4ae05e5d7a5b737d4104419165b98e4d486c5e2208b30b95173f2793141ef897f23f22c87c15f80daf8dcc2dbb4327c2fdf3288d56acd80')
b2sums=('3bd8880386f0b2f73d2587e9ad5803ded1e107be4f4b8fe75f17453a3e98624f9f34c15a2ef11a2bb8362809100b6d9ce4fd6e1d350af164b3cf954cae7653db')

prepare() {
  cd $pkgname-$pkgver
  sed 's|/usr/local|/usr|' -i resources/$pkgname.service
  cargo fetch --locked --target "$(rustc --print host-tuple)"
}

build() {
  cd $pkgname-$pkgver
  cargo build --frozen --release --features systemd
}

check() {
  cd $pkgname-$pkgver
  export XDG_RUNTIME_DIR="$(mktemp -d)"
  cargo test --frozen
}

package() {
  cd $pkgname-$pkgver
  install -vDm 755 target/release/$pkgname -t "$pkgdir/usr/bin/"
  install -vDm 644 resources/$pkgname.service -t "$pkgdir/usr/lib/systemd/user/"
  install -vDm 644 LICENSE -t "$pkgdir/usr/share/licenses/$pkgdir/"
}
