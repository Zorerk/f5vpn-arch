# Maintainer: Zach Hoffman <zach@zrhoffman.net>
pkgname=f5vpn
pkgver=7190.2020.0221.1
pkgrel=3
pkgdesc='VPN client using the Point-to-Point Protocol to connect to F5Networks BIG-IP APM 13.0'
arch=('x86_64')
source=('LICENSE')
source_x86_64=("linux_f5vpn-${pkgver}.x86_64.rpm::https://connect.healthsystem.virginia.edu/public/download/linux_f5vpn.x86_64.rpm")
sha256sums=('a8f4b5d965dc0279dd5173109892251ce5d107d9912836e6d83a9b6896eb19a1')
sha256sums_x86_64=('d66d2a65d019a22397b0210805092d4cd582d4db1c89d47c16b191dc847da14a')
md5sums=('2508fc5e24d46163844dba9534fe7924')
md5sums_x86_64=('561f1e54f3ecc4bfe371ac6af60f8c16')
depends=(icu openssl qt5-base qt5-webkit)
url='https://support.f5.com/csp/article/K32311645#link_04_05'
license=('commercial')

package() {
    (
    cd "${srcdir}/opt/f5/vpn"
    chmod u+s svpn # f5vpn should not be run as non-root, but it calls svpn which must be run as root
    install -Dm644 "com.f5.${pkgname}.desktop" "${pkgdir}/usr/share/applications/com.f5.${pkgname}.desktop"
    install -dm755 "${pkgdir}/usr/bin/"
    install -dm755 "${pkgdir}/usr/local/lib/F5Networks/SSLVPN/var/run" # For svpn.pid
    for executable in $pkgname svpn tunnelserver; do
        ln -s "/opt/f5/vpn/${executable}" "${pkgdir}"/usr/bin/${executable}
    done

    # Use system Qt libraries
    for library in lib/*.so.*; do
        ln -sf "/usr/${library%%.so.*}.so" "$library"
    done

    # Use system Qt libraries
    for plugin in platforms/*.so; do
        ln -sf "/usr/lib/qt/plugins/${plugin}" "$plugin"
    done

    for resolution in 16 24 32 48 64 96 128 256 512 1024; do
        install -Dm644 "logos/${resolution}x${resolution}.png" \
                        "${pkgdir}/usr/share/icons/hicolor/${resolution}x${resolution}/apps/${pkgname}.png"
    done
    )
    install -Dm644 'LICENSE' "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
    cp -a opt "${pkgdir}"

}
