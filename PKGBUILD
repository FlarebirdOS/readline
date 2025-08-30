pkgname=readline
pkgver=8.3
pkgrel=2
pkgdesc="GNU readline library"
arch=('x86_64')
url="https://tiswww.case.edu/php/chet/readline/rltop.html"
license=('GPL-3.0-only')
depends=(
    'glibc'
    'ncurses'
)
backup=(etc/inputrc)
options=('!emptydirs')
source=(https://ftp.gnu.org/gnu/readline/${pkgname}-${pkgver}.tar.gz
    inputrc)
sha256sums=(fe5383204467828cd495ee8d1d3c037a7eba1389c22bc6a041f627976f9061cc
    e52830a166d31d798a1aeab5087efdb5483c74942bf6c7910af34b3acf95c14c)

prepare() {
    cd ${pkgname}-${pkgver}

    sed -i '/MV.*old/d' Makefile.in
    sed -i '/{OLDSUFF}/c:' support/shlib-install

    sed -i 's/-Wl,-rpath,[^ ]*//' support/shobj-conf
}

build() {
    cd ${pkgname}-${pkgver}

    local configure_args=(
        --disable-static
        --with-curses
        --docdir=/usr/share/doc/${pkgname}-${pkgver}
        ${configure_options}
    )

    CFLAGS="${CFLAGS} -fPIC"

    ./configure "${configure_args[@]}"

    make SHLIB_LIBS="-lncursesw"
}

package() {
    cd ${pkgname}-${pkgver}

    make DESTDIR=${pkgdir} SHLIB_LIBS="-lncursesw" install

    install -vm644 doc/*.{ps,pdf,html,dvi} ${pkgdir}/usr/share/doc/${pkgname}-${pkgver}

    install -vDm644 ${srcdir}/inputrc ${pkgdir}/etc/inputrc
}
