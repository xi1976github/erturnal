# Maintainer: xi76 <xi@jabbxi.de>

_pkgname=eturnal
pkgname=eturnal-git
pkgver=1.4.5.10.g07e2d05
pkgrel=1
pkgdesc="eturnal STUN/TURN Server"
arch=('x86_64')
url="https://github.com/processone/${_pkgname}"
license=('APACHE')

provides=("eturnal=$pkgver")
conflicts=('eturnal')

makedepends=(
         'git' 'rebar3' 'tar'
        )
depends=( 'erlang-nox' 'libyaml' 'bash' )
backup=('etc/eturnal.yml')

source=("git+https://github.com/processone/${_pkgname}.git" "sysuser.conf" "tmpfile.conf")

sha256sums=('SKIP'
	   '4eb71efbd999d9f890238a22dd3b9730a68f2fc8239adb2275e7b4331117e40a'
	   'cb5463d7a4723b53f6be1d1416f2364aa7bfc12128eb004b0a9d76bb2b415900'
   )

pkgver() {
    cd "$_pkgname"
     git describe --long --tags | sed 's/^v//; s/-/./g'
}

prepare() {
    cd "$_pkgname"
    #sed -i 's/opt\/ieturnal/usr/' build.config
}

build() {
    cd "$_pkgname"
    rebar3 as prod tar
    COMPILED=$(ls _build/prod/rel/eturnal/*.gz -t|head -1)
    echo $COMPILED
    mkdir -p tar
    cd tar
    tar -xzf "../$COMPILED"

}
package() {
  cd "$_pkgname"/tar
  #sed -i 's/opt\/eturnal/usr/g' etc/systemd/system/eturnal.service	
  install -Dm0644 etc/systemd/system/eturnal.service "${pkgdir}"/usr/lib/systemd/system/eturnal.service
  install -Dm0644 etc/eturnal.yml -t "${pkgdir}"/etc
  #install -Dm0755 bin/eturnalctl "${pkgdir}"/usr/bin/eturnalctl
  #install -Dm0755 bin/eturnal "${pkgdir}"/usr/bin/eturnal
  #install -Dm0644 doc/* -t "${pkgdir}"/usr/share/doc/eturnal
  install -Dm0644 "${srcdir}"/sysuser.conf "${pkgdir}"/usr/lib/sysusers.d/eturnal.conf
  install -Dm0644 "${srcdir}"/tmpfile.conf "${pkgdir}"/usr/lib/tmpfiles.d/eturnal.conf
  install -d "${pkgdir}"/opt/eturnal
  cp -r * "${pkgdir}"/opt/eturnal

}
