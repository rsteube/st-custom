pkgname=st-custom
_pkgname=st
pkgver=0.8.5.r0.g7fb0c0c
pkgrel=1
epoch=1
pkgdesc='Simple virtual terminal emulator for X'
url='https://st.suckless.org/'
arch=('i686' 'x86_64')
license=('MIT')
options=('zipman')
#depends=('libxft' 'nerd-fonts-meslo')
depends=('libxft')
makedepends=('ncurses' 'libxext' 'git')
provides=("${_pkgname}")
conflicts=("${_pkgname}")

source=(
        "config.h"
        "git://git.suckless.org/st#tag=0.8.5"
        "https://st.suckless.org/patches/alpha/st-alpha-20220206-0.8.5.diff"
        "https://st.suckless.org/patches/scrollback/st-scrollback-0.8.5.diff"
        "https://st.suckless.org/patches/scrollback/st-scrollback-mouse-20220127-2c5edf2.diff"
        "https://st.suckless.org/patches/scrollback/st-scrollback-mouse-altscreen-20220127-2c5edf2.diff"
        "https://st.suckless.org/patches/fix_keyboard_input/st-fix-keyboard-input-20180605-dc3b5ba.diff"
        "st-externalpipe-0.8.5-custom.diff"
        "https://st.suckless.org/patches/externalpipe/st-externalpipe-eternal-0.8.3.diff")

md5sums=('1cc3ad563dd65b1fd495ac7010b49285'
         'SKIP'
         '2bd6801c2abd29c86e7b5908b043708d'
         '2540178ff4c1ead78a7249d1d9939d52'
         '988cbd4c8612dbe1b6f1845d06cfc6f0'
         'd740e376ed70ed719f5d68e44755e861'
         'b97e0610618e556ec3bcb11fcce7d039'
         '664c0017537fbd8c8295948c561c5e89'
         '17df592890b398f6425e58eb4174b357')

pkgver() {
	cd "${_pkgname}"
	git describe --long --tags | sed 's/\([^-]*-g\)/r\1/;s/-/./g'
}

prepare() {
	local file
	cd "${_pkgname}"
	
	# Skip terminfo which conflicts with ncurses
	sed -i '/tic /d' Makefile

	for file in "${source[@]}"; do
		if [[ "$file" == "config.h" ]]; then
			# add config.h if present in source array
			# Note: this supersedes the above sed to config.def.h
			cp "$srcdir/$file" .
		elif [[ "$file" == *.diff || "$file" == *.patch ]]; then
			# add all patches present in source array
			echo "Applying patch $(basename $file)..."
			patch -Np1 <"$srcdir/$(basename ${file})"
		fi
	done

}

build() {
	cd "${_pkgname}"
	make X11INC=/usr/include/X11 X11LIB=/usr/lib/X11
}

package() {
	cd "${_pkgname}"
	make PREFIX=/usr DESTDIR="${pkgdir}" install
	install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
	install -Dm644 README "${pkgdir}/usr/share/doc/${pkgname}/README"
}
