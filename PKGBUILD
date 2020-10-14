# Maintainer: Zach Hoffman <zach@zrhoffman.net>
pkgname=f5vpn
pkgver=7183.2020.0108.1
pkgrel=1
pkgdesc='VPN client using the Point-to-Point Protocol to connect to F5Networks BIG-IP APM 16.0'
arch=('x86_64')
source=('LICENSE'
        'no-desktop-file-dbus.patch')
# Hashes match linux_f5vpn.x86_64.rpm from APM Clients version 7.1.83 (apmclients-7183.2020.0108.1-1.iso) from downloads.f5.com
source_x86_64=("linux_${pkgname}-${pkgver}-${pkgrel}.x86_64.rpm::https://vpn.ljmu.ac.uk/public/download/linux_${pkgname}.x86_64.rpm")
depends=(icu openssl qt5-base qt5-webkit)
url='https://support.f5.com/csp/article/K32311645#link_04_05'
license=('commercial')

sha256sums=('5ce1e05a353e5a95c2fa5f7a4411c62c58a54dbd867630c5164aa46c5525e2a6'	
            '5ce1e05a353e5a95c2fa5f7a4411c62c58a54dbd867630c5164aa46c5525e2a6')	
sha256sums_x86_64=('5ce1e05a353e5a95c2fa5f7a4411c62c58a54dbd867630c5164aa46c5525e2a6')	
md5sums=('475813581c09861c69e5e7376909e3bb'	
         '130ef2376ad4581cc91a11814c00d948')	
md5sums_x86_64=('475813581c09861c69e5e7376909e3bb')

package() {
    (
    cd "${srcdir}/opt/f5/vpn"

    patch -i "${srcdir}/no-desktop-file-dbus.patch" # Desktop file does not work with Dbus enabled

    chmod u+s svpn # f5vpn should not be run as root, but it calls svpn which must be run as root
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
