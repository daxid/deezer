# Maintainer: Sibren Vasse <arch@sibrenvasse.nl>
# Contributor: Ilya Gulya <ilyagulya@gmail.com>
<<<<<<< HEAD
pkgname="deezer2"
pkgver=5.30.70
=======
pkgname="deezer"
pkgver=5.30.391
>>>>>>> main
pkgrel=1
pkgdesc="A proprietary music streaming service"
arch=('any')
url="https://www.deezer.com/"
license=('custom:"Copyright (c) 2006-2022 Deezer S.A."')
depends=('electron13-bin')
provides=('deezer')
makedepends=('p7zip' 'asar' 'prettier' 'imagemagick' 'npm' 'nodejs')
source=("$pkgname-$pkgver-setup.exe::https://www.deezer.com/desktop/download/artifact/win32/x86/$pkgver"
    "$pkgname.desktop"
    deezer2
    remove-kernel-version-from-user-agent.patch
    avoid-change-default-texthtml-mime-type.patch
    fix-isDev-usage.patch
    start-hidden-in-tray.patch
    quit.patch)
sha256sums=('8af76ddb6ae60d29240a15a9dd8ae60ae3ad19b2d62043dc6ead5d7a6d56d722'
            'f8a5279239b56082a5c85487b0c261fb332623f27dac3ec8093458b8c55d8d99'
            'b464dbfc0d426730259ce2abc660960ecc6a73bbef838b8fbbd15deb2f5dce53'
            'ec87bbcc5a615c61c78bf117889d5b697a2174150722b1318205ad1c903286f2'
            '357b1e208aa58353a0fd176318b4628a7bed3210686e35f8d30399c750c99b0d'
            '731d25269ed260b386ad40937e38b6f56634cf7a13e2a98a8eaf53a778026161'
            '505bc0363443f8a921361efb5f204a15afa7773e57fa386c2221ba1609b896c8'
            '78d26c08c234594eeba0ac68c95612a8c01ea4026f34e0141e8a997287b0af1b')

prepare() {
    # Extract app from installer
    7z x -so $pkgname-$pkgver-setup.exe "\$PLUGINSDIR/app-32.7z" >app-32.7z
    # Extract app archive
    7z x -y -bsp0 -bso0 app-32.7z

    # Extract png from ico container
    convert resources/win/app.ico resources/win/deezer.png

    cd resources/
    asar extract app.asar app

    cd "$srcdir/resources/app"
    mkdir -p resources/linux/
    install -Dm644 "$srcdir/resources/win/systray.png" resources/linux/

    prettier --write "build/*.js"

    local src
    for src in "${source[@]}"; do
      src="${src%%::*}"
      src="${src##*/}"
      [[ $src = *.patch ]] || continue
      echo "Applying patch ${src}..."
      patch -Np1 < "${srcdir}/${src}"
    done

    cd "$srcdir/resources/"
    asar pack app app.asar
}

package() {
    mkdir -p "$pkgdir/usr/share/deezer2/"
    mkdir -p "$pkgdir/usr/share/applications/"
    mkdir -p "$pkgdir/usr/bin/"

    install -Dm644 resources/app.asar "$pkgdir/usr/share/deezer2/"
    install -Dm644 "$pkgname.desktop" "$pkgdir/usr/share/applications/"
    install -Dm755 "$pkgname" "$pkgdir/usr/bin/"
}
