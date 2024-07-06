# Maintainer: Nick B <devel.niks@gmail.com>
# Contributor: David Parrish <daveparrish@tutanota.com>
# Contributor: Felix Golatofski <contact@xdfr.de>

pkgname=bisq-latest
_pkgname=bisq
pkgver=1.9.17
pkgrel=1
pkgdesc="Cross-platform desktop application that allows users to trade national currency (dollars, euros, etc) for bitcoin without relying on centralized exchanges"
arch=('any')
url="https://bisq.network"
license=('AGPL3')
depends=('jdk11-openjdk')
makedepends=('jdk11-openjdk' 'git')
source=(
	"git+https://github.com/bisq-network/bisq#branch=release/v$pkgver"
	"bisq.desktop"
)
sha256sums=(
        'SKIP'
        '687d643fbe84660c3ebfe6270de98214f2e3ea791cb1d07d96d7ed889d61d406'
)

_binname=Bisq
conflicts=("bisq-bin" "bisq")
provides=("bisq")

build() {
  cd "${srcdir}/${_pkgname}" || exit
  msg2 "Building bisq..."
  sed -i '/vendor = JvmVendorSpec.AZUL/d' build-logic/commons/src/main/groovy/bisq.java-conventions.gradle
  sed -i '/implementation = JvmImplementation.VENDOR_SPECIFIC/d' build-logic/commons/src/main/groovy/bisq.java-conventions.gradle
  ./gradlew clean :desktop:build -Dorg.gradle.java.home=/usr/lib/jvm/java-11-openjdk -x test
}

package() {
  # Install executable.
  install -d "${pkgdir}/opt/bisq"
  cp -r "${srcdir}/${_pkgname}/desktop/build/app/"* "${pkgdir}/opt/bisq"
  cp -r "${srcdir}/${_pkgname}/bisq-desktop" "${pkgdir}/opt/bisq/"
  install -d "${pkgdir}/usr/bin"
  ln -s "/opt/bisq/bisq-desktop" "${pkgdir}/usr/bin/bisq-desktop"

  # Install desktop launcher.
  install -Dm644 bisq.desktop "${pkgdir}/usr/share/applications/bisq.desktop"
  install -Dm644 "${srcdir}/${_pkgname}/desktop/package/linux/icon.png" "${pkgdir}/usr/share/pixmaps/bisq.png"
}
