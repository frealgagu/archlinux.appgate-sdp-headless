# Maintainer: Fredy García <frealgagu at gmail dot com>
# Contributor: Pawel Mosakowski <pawel at mosakowski dot net>

pkgname=appgate-sdp-headless
pkgver=6.4.0
pkgrel=1
pkgdesc="Appgate SDP (Software Defined Perimeter) headless client (It does not support 2FA.)"
arch=("x86_64")
url="https://www.${pkgname%%-*}.com/support/software-defined-perimeter-support"
license=("custom" "custom:commercial")
depends=("libsecret" "libxss" "nodejs" "nss" "python-dbus" "python-distro")
optdepends=(
  "bash-completion: allows bash completion for scripts"
  "dnsmasq: dns resolver for systems without systemd-resolved"
)
provides=("${pkgname%-*}")
conflicts=("${pkgname%-*}")
backup=("etc/appgate.conf" "etc/dbus-1/system.d/appgate.conf")
options=(staticlibs !strip !emptydirs)
install="${pkgname}.install"
source=(
  "https://bin.${pkgname%-*}.com/${pkgver%.*}/client/${pkgname}_${pkgver}_amd64.deb"
  "${pkgname%%-*}service.service.patch"
  "10-appgate-tun.network"
)
sha256sums=(
  "60633cff0107060467832e85114ce353e6ac1e0500982327fa0eb52e3f7f2b9f"
  "8c2986036f8ca9b8c9bca5861efc98322d9bd1b77951a850059e86b1b84f10c6"
  "2eb0daa10429e67d703cceccd34069da3044d99c5652658ec73c7a01c88b64e9"
)

prepare() {
  mkdir "${srcdir}/${pkgname}"
  cd "${srcdir}/${pkgname}"

  bsdtar -xf "${srcdir}/data.tar.xz" -C .

  patch -Np1 -i "${srcdir}/${pkgname%%-*}service.service.patch"

  # Remove unnecessary .deb related directory
  rm -rf "${srcdir}/${pkgname}/etc/init.d"
}

package() {
  # Install application files
  cp -dpr "${srcdir}/${pkgname}/"{etc,opt,usr} "${pkgdir}"

  # Install service files
  install -dm755 "${pkgdir}/usr/lib/systemd/system"
  install -Dm644 "${srcdir}/${pkgname}/lib/systemd/system/"* "${pkgdir}/usr/lib/systemd/system/"
  mv "${pkgdir}/usr/sbin" "${pkgdir}/usr/bin"

  # Make systemd-networkd not manage tun interfaces
  install -dm755 "${pkgdir}/usr/lib/systemd/network"
  install -Dm644 "${srcdir}/10-appgate-tun.network" "${pkgdir}/usr/lib/systemd/network/"

  # Install license files
  install -Dm644 "${srcdir}/${pkgname}/usr/share/doc/${pkgname/-sdp/}/copyright" "${pkgdir}/usr/share/licenses/${pkgname}/copyright"
}
