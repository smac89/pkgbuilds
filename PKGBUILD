# Maintainer: Mubashshir <ahmubashshir@gmail.com>
# from: pypi
# what: trakt-scrobbler


pkgname=trakt-scrobbler
pkgver=1.6.0
pkgrel=3
pkgdesc="Automatically scrobble TV show episodes and movies you are watching to Trakt.tv! It keeps a history of everything you've watched!"

arch=(any)
url=https://github.com/iamkroot/trakt-scrobbler
license=(GPL2)
depends=(
	"systemd"
	'python-requests<3.0.0' #auto-deps
	'python-requests>=2.25.1' #auto-deps
	'python-urllib3<2.0.0' #auto-deps
	'python-urllib3>=1.26.0' #auto-deps
	'python-guessit<4.0.0' #auto-deps
	'python-guessit>=3.3.1' #auto-deps
	'python-appdirs<2.0.0' #auto-deps
	'python-appdirs>=1.4.4' #auto-deps
	'python-cleo<0.9.0' #auto-deps
	'python-cleo>=0.8.1' #auto-deps
	'python-confuse<3.0' #auto-deps
	'python-confuse>=2.0' #auto-deps
	'python-urlmatch<2.0.0' #auto-deps
	'python-urlmatch>=1.0.1' #auto-deps
	'python-pydantic<2.0.0' #auto-deps
	'python-pydantic>=1.10.2' #auto-deps
	'python-toml<0.11.0' #auto-deps
	'python-toml>=0.10.2' #auto-deps
)

makedepends=(
    python-poetry-core
    python-build
    python-installer
    python-wheel
    go-md2man
    gzip
)
source=(
    "$pkgname-$pkgver.tar.gz::https://files.pythonhosted.org/packages/source/${pkgname::1}/${pkgname//-/_}/${pkgname//-/_}-$pkgver.tar.gz"
    "trakts-man.md"
)
sha256sums=('a9947e0cf84039f2718aef133bb625a9945f42c2ddbb879432125c0ee6c4a433'
            '81c3fb93bf01c0e6c0bbc9b2ef853da3f691bc3c50b4a87a68072b11ba72691c')

prepare() {
    cd "$srcdir/${pkgname//-/_}-$pkgver"

	sed -i 's/cleo\s/cleo_v0 /' \
		trakt_scrobbler/console.py \
		trakt_scrobbler/commands/command.py
}

build() {
    cd "$srcdir/${pkgname//-/_}-$pkgver"
    python -m build --no-isolation --wheel
    go-md2man -in "$srcdir/trakts-man.md" 2>/dev/null|gzip -n > trakts.1.gz
}

package()
{
    cd "$srcdir/${pkgname//-/_}-$pkgver"
    _py=$(python --version)
    _py=${_py%%.*}

    python -m installer --destdir="$pkgdir" \
		"dist/${pkgname//-/_}-${pkgver%.r*}-py${_py##* }-none-any.whl"
    install -Dm644 "trakts.1.gz" "$pkgdir/usr/share/man/man1/trakts.1.gz"
    install -Dm755 "completions/trakts.zsh" "$pkgdir/usr/share/zsh/site-functions/_trakts"
}
