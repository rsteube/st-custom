pkgname=st-custom
_pkgname=st
pkgver=0.8.1
pkgrel=1
epoch=1
pkgdesc='Simple virtual terminal emulator for X'
url='https://st.suckless.org/'
arch=('i686' 'x86_64')
license=('MIT')
options=('zipman')
depends=('libxft' 'nerd-fonts-complete-mono-glyphs')
makedepends=('ncurses' 'libxext' 'git')
provides=("${_pkgname}")
conflicts=("${_pkgname}")

source=("git://git.suckless.org/st#tag=0.8.1"
        "config.h"
        "https://st.suckless.org/patches/alpha/st-alpha-20180616-0.8.1.diff"
        "https://st.suckless.org/patches/scrollback/st-scrollback-0.8.diff"
        "https://st.suckless.org/patches/scrollback/st-scrollback-mouse-0.8.diff"
        "https://st.suckless.org/patches/scrollback/st-scrollback-mouse-altscreen-0.8.diff"
        "https://st.suckless.org/patches/externalpipe/st-externalpipe-0.8.1.diff")

md5sums=('SKIP'
         'SKIP'
         'SKIP'
         'SKIP'
         'SKIP'
         'SKIP'
         'SKIP')

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
