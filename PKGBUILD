# Maintainer: Danny Navarro <j@dannynavarro.net>
pkgname=cabal-dev-git
pkgver=20130305
pkgrel=1
pkgdesc="Manage sandboxed Haskell build environments"
arch=('i686' 'x86_64')
url="https://github.com/creswick/${_gitname}"
depends=('cabal-install' 'gmp' 'zlib')
makedepends=(
    'git'
    'ghc'
    'haskell-transformers'
    'haskell-mtl'
    'haskell-http'
    'haskell-network'
    'haskell-tar'
    'haskell-zlib'
    'haskell-setenv'
)
license=('custom:BSD3')
provides=('cabal-dev')
conflicts=('cabal-dev')

_gitroot="git://github.com/creswick/cabal-dev.git"
_gitname="cabal-dev"
_archname="cabal-dev"

build() {
    cd ${srcdir}/
    msg "Connecting to GIT server...."

    if [ -d ${_gitname} ] ; then
        cd ${_gitname} && git pull origin
        msg "The local files are updated."
    else
        git clone ${_gitroot} ${_gitname} -b master
    fi

    msg "GIT checkout done or server timeout"
    msg "Starting make..."

    rm -rf "${srcdir}/${_gitname}-build"
    git clone "${srcdir}/${_gitname}" "${srcdir}/${_gitname}-build"
    cd "${srcdir}/${_gitname}-build"

    runhaskell Setup configure -O --enable-split-objs --enable-shared \
       --prefix=/usr --enable-executable-stripping \
       --docdir=/usr/share/doc/${_archname} \
       --datasubdir=/usr/share/${_archname}
    runhaskell Setup build
}
package() {
    cd "${srcdir}/${_gitname}-build"
    runhaskell Setup copy --destdir=${pkgdir}
    install -D -m644 LICENSE ${pkgdir}/usr/share/licenses/${_archname}/LICENSE
    rm -fr ${pkgdir}/usr/share/doc
}
