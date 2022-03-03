# Maintainer: Mubashshir <ahmubashshir@gmail.com>
# from: pypi
# what: trakt-scrobbler
# deps: pyproject

pkgname=trakt-scrobbler
pkgver=1.4.2
pkgrel=2
pkgdesc="Automatically scrobble TV show episodes and movies you are watching to Trakt.tv! It keeps a history of everything you've watched!"

arch=(any)
url=https://github.com/iamkroot/trakt-scrobbler
license=(GPL2)
depends=(
	"systemd"
)

makedepends=(
    python-setuptools
    go-md2man
    gzip
)
source=(
    "$pkgname-$pkgver.tar.gz::https://files.pythonhosted.org/packages/source/${pkgname::1}/$pkgname/$pkgname-$pkgver.tar.gz"
    "trakts-man.md"
)
sha256sums=('66523cbae25d211cdb7306034b3e2e545cd690bf6834188f43f3fdbb69b4fe49'
            '81c3fb93bf01c0e6c0bbc9b2ef853da3f691bc3c50b4a87a68072b11ba72691c')

build() {
    cd "$srcdir/$pkgname-$pkgver"
    python setup.py build
    go-md2man -in "$srcdir/trakts-man.md" 2>/dev/null|gzip -n > trakts.1.gz
}

package()
{
    cd "$srcdir/$pkgname-$pkgver"
    python setup.py install --root="$pkgdir" --optimize=1 --skip-build
    install -Dm644 "trakts.1.gz" "$pkgdir/usr/share/man/man1/trakts.1.gz"
    install -Dm755 "completions/trakts.zsh" "$pkgdir/usr/share/zsh/site-functions/_trakts"
}
