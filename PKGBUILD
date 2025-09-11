# Maintainer: TBK <aur at jjtc dot eu>
# Contributor: TBK <aur at jjtc dot eu>

pkgname=termius
pkgver=9.31.0
pkgrel=1
pkgdesc="Desktop SSH Client"
url="https://www.termius.com/"
arch=('x86_64')
license=('custom')
depends=('at-spi2-core' 'dbus' 'e2fsprogs' 'expat' 'gtk3' 'keyutils' 'libbsd' 'libnotify' 'libsecret' 'libxss' 'libxtst' 'nss' 'util-linux-libs' 'xdg-utils')
optdepends=('libappindicator-gtk3')
makedepends=('squashfs-tools')
# Get latest version + link from https://snapcraft.io/termius-app || snap info termius-app || run the following
# curl -H 'X-Ubuntu-Series: 16' https://api.snapcraft.io/api/v1/snaps/details/termius-app | jq '.download_url' -r
# curl -H 'X-Ubuntu-Series: 16' https://api.snapcraft.io/api/v1/snaps/details/termius-app | jq '.version' -r
source=(
    "$pkgname-$pkgver.snap::https://api.snapcraft.io/api/v1/snaps/download/WkTBXwoX81rBe3s3OTt3EiiLKBx2QhuS_237.snap"
    "termius.desktop"
    "tos.html"
)
sha512sums=('50c73b239686c05e499ac5c93ce5a72b44692b6db8c170106caefaf8dd858d91a4b702a11d8217498ceadfdec097f980ffbd55936a1b2d608d9d7bb5bd15e434'
            'f1ce576d42a624842c9d08807c11580421b708b4bd7fac3aa9874769735df87566012e8fc0f993b08618f31d2f38588cd83d8572a2700f35d42e2761984ca5d0'
            '6ac7c082d1adba92dd911f46f9926f702be0f92a9843e6252364477d81364569eeeee9b37170a6d9000fde644588734cb6a11d165fc0aff3dbfbcd6ad353ca96')

prepare() {
    mkdir -p $pkgname
    unsquashfs -f -d $pkgname $pkgname-$pkgver.snap
}

package() {
    # Option 1 - copy only the needed files ~183 MiB
    mkdir -p "$pkgdir"/opt/$pkgname

    cd "$srcdir"/$pkgname

    cp -r \
        chrome_100_percent.pak \
        chrome_200_percent.pak \
        chrome_crashpad_handler \
        icudtl.dat \
        libEGL.so \
        libffmpeg.so \
        libGLESv2.so \
        libvk_swiftshader.so \
        libvulkan.so.1 \
        locales \
        resources \
        resources.pak \
        termius-app \
        v8_context_snapshot.bin \
        vk_swiftshader_icd.json \
        "$pkgdir"/opt/$pkgname
    
    cd "$srcdir"
    # Option 2 - copy all files from the .snap file ~503 MiB
    #mkdir -p "$pkgdir"/opt/
    #cp -r "$srcdir"/$pkgname "$pkgdir"/opt/$pkgname

    find "$pkgdir"/opt/$pkgname/ -type f -exec chmod 644 {} \;
    chmod 755 "$pkgdir"/opt/$pkgname/termius-app
    chmod 755 "$pkgdir"/opt/$pkgname/chrome_crashpad_handler

    mkdir -p "$pkgdir"/usr/bin
    ln -sf /opt/$pkgname/termius-app "$pkgdir"/usr/bin/$pkgname
    install -Dm0644 tos.html "$pkgdir"/usr/share/licenses/$pkgname/tos.html
    install -Dm0644 $pkgname.desktop "$pkgdir"/usr/share/applications/$pkgname.desktop
    install -Dm0644 $pkgname/meta/gui/icon.png "$pkgdir"/usr/share/pixmaps/$pkgname.png
}
