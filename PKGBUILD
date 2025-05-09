# Maintainer: Vescrity <vescrity@foxmail.com>
pkgname=wfrc-git
pkgver="0"
pkgrel=1
pkgdesc='Easy screen recorder using wf-recorder'
arch=(any)
depends=(
    wf-recorder
    bash
    grep
    slurp
    libnotify
    libpulse
    wl-clipboard
)
optdepends=(
)
makedepends=(
  git
)
provides=(wfrc)
conflicts=()
source=('git+https://github.com/Vescrity/wfrc')
sha256sums=('SKIP')
pkgver() {
    cd "$srcdir/wfrc"
    printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short=7 HEAD)"
}
package() {
  cd "$srcdir/wfrc"
  install -d "${pkgdir}/usr/bin"
  install -Dm755 "wfrc" "${pkgdir}/usr/bin/wfrc"
}
