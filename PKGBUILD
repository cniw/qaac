# Maintainer: Wachid Adi Nugroho <wachidadinugroho.maya@gmail.com>

pkgname=qaac
pkgver=2.68
pkgrel=1
pkgdesc="command line AAC/ALAC encoder frontend based on Apple encoder"
arch=(i686 x86_64)
url="http://sites.google.com/site/qaacpage"
license=('custom')
depends=(wine)
makedepends=(p7zip)
source=("https://sites.google.com/site/qaacpage/cabinet/qaac_${pkgver}.zip"
        "https://secure-appldnld.apple.com/QuickTime/031-43075-20160107-C0844134-B3CD-11E5-B1C0-43CA8D551951/QuickTimeInstaller.exe"
        "x32DLLs_20190107.zip")
noextract=(qaac_${pkgver}.zip QuickTimeInstaller.exe x32DLLs_20190107.zip)
options=(!strip)
sha256sums=("8067826564d182a239a2347b40d52369c4a378b7df7918bd156138bf904168d0"
            "56eff77b029b5f56c47d11fe58878627065dbeacbc3108d50d98a83420152c2b"
            "afbe6fff760a169c4144a137403b2036618d67094f2302f3a57650712dd201d9")

_qaacfiles=(qaac_${pkgver}/x86/*)
_coreaudiofiles=(CoreAudioToolbox.dll CoreFoundation.dll)
_coreaudiodeps=(ASL.dll icudt*.dll libdispatch.dll libicu*.dll msvcr80.dll* msvcp80.dll* objc.dll pthreadVC2.dll)
_x32dlls=(x32DLLs_20190107/*.dll)


package() {
  install -d -m755 ${pkgdir}/usr/share/${pkgname}

  7z e ${srcdir}/qaac_${pkgver}.zip -o${pkgdir}/usr/share/${pkgname} ${_qaacfiles[@]}

  7z e ${srcdir}/QuickTimeInstaller.exe AppleApplicationSupport.msi -o${srcdir}
  7z e ${srcdir}/AppleApplicationSupport.msi -o${pkgdir}/usr/share/${pkgname} ${_coreaudiofiles[@]} ${_coreaudiodeps[@]}
  mv ${pkgdir}/usr/share/${pkgname}/msvcr80* ${pkgdir}/usr/share/${pkgname}/msvcr80.dll
  mv ${pkgdir}/usr/share/${pkgname}/msvcp80* ${pkgdir}/usr/share/${pkgname}/msvcp80.dll

  7z e ${srcdir}/x32DLLs_20190107.zip -o${pkgdir}/usr/share/${pkgname} ${_x32dlls[@]}

  find ${pkgdir}/usr/share/${pkgname} -type d -exec chmod 755 "{}" \;
  find ${pkgdir}/usr/share/${pkgname} -type f -exec chmod 644 "{}" \;

  install -d -m755 ${pkgdir}/usr/bin

  for _apps in qaac refalac; do cat > ${srcdir}/${_apps} << EOL
#!/bin/sh
export WINEDLLOVERRIDES="mshtml="
WINEDEBUG=-all wine /usr/share/${pkgname}/${_apps}.exe "\$@" 
EOL
  done

  install -m755 ${srcdir}/qaac ${pkgdir}/usr/bin
  install -m755 ${srcdir}/refalac ${pkgdir}/usr/bin
  
  rm -f ${srcdir}/{AppleApplicationSupport.msi,qaac,refalac}
}
