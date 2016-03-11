# Maintainer:  Benjamin Maisonnas <ben at wainei dot net>

# Based on PKGBUILD from https://aur.archlinux.org/packages/canon-pixma-mg5500-complete
# Scanner icon source: http://openiconlibrary.sourceforge.net/gallery2/open_icon_library-full/icons/png/64x64/devices/scanner-3.png

pkgname=canon-pixma-mg5650-complete
pkgver=1.00
pkgrel=1
pkgdesc="Complete stand alone driver set (printing and scanning) for Canon Pixma MG5650 series"
url='http://www.canon-europe.com/Support/Consumer_Products/products/Fax__Multifunctionals/InkJet/PIXMA_MG_series/PIXMA_MG5650.aspx'
arch=('i686' 'x86_64')
license=('custom')
install="${pkgname}.install"
depends_x86_64=('lib32-libpng12' 'lib32-libusb-compat' 'lib32-libtiff4' 'lib32-libxml2')
depends_i686=('popt' 'libpng12' 'libusb-compat' 'libtiff4' 'libxml2' 'gtk2')


makedepends=('deb2targz' 'sed')
source=('http://gdlp01.c-wss.com/gds/5/0100006265/01/cnijfilter2-5.00-1-deb.tar.gz'
         'http://gdlp01.c-wss.com/gds/1/0100006271/01/scangearmp2-3.00-1-deb.tar.gz'
         "${pkgname}.license"
         "${pkgname}-scangear.desktop"
         "${pkgname}-scangear-icon.png")
md5sums=('433e5d53041067deb90584397daec07d'
         'b6ccbe9bf884efb9f8ae1aa0fd390609'
         'd8faf0e75ee3d1989eed39d55cb3e8fb'
         '582545d30ad1f76f55fd7cefda817e63'
         '3feefd413092d7152f47b7451ba79efe')

_printDrvSrc='cnijfilter2-5.00-1-deb'
_scanDrvSrc='scangearmp2-3.00-1-deb'

if [ "${CARCH}" = "x86_64" ]; then
  _printDrvDeb='cnijfilter2_5.00-1_amd64'
  _scanDrvDeb='scangearmp2_3.00-1_amd64'
else
  _printDrvDeb='cnijfilter2_5.00-1_i386'
  _scanDrvDeb='scangearmp2_3.00-1_i386'
fi

_ppdFile="canonmg5600.ppd"

build() {
  cd ${srcdir}

  # Unpack driver source files
  tar xvzf ${_printDrvSrc}.tar.gz
  tar xvzf ${_scanDrvSrc}.tar.gz

  rm -v *.tar.gz
}

package() {
  cd ${pkgdir}

  # Unpack debian installer files
  cp "${srcdir}/${_printDrvSrc}/packages/${_printDrvDeb}.deb" .
  cp "${srcdir}/${_scanDrvSrc}/packages/${_scanDrvDeb}.deb" .

  deb2targz "${_printDrvDeb}.deb"
  deb2targz "${_scanDrvDeb}.deb"

  rm -v *.deb

  tar xzvf "${_printDrvDeb}.tar.gz"
  tar xzvf "${_scanDrvDeb}.tar.gz"

  rm -v *.tar.gz

  # Move ppd file to where cups expects
  install -vDm 644 "${pkgdir}/usr/share/ppd/${_ppdFile}" "${pkgdir}/usr/share/cups/model/${_ppdFile}"

  rm -vrf ${pkgdir}/usr/share/ppd

  # Install custom license file
  install -vDm 644 "${srcdir}/${pkgname}.license" "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"

  # Install entry in application menu, including icon
  install -vDm 644 "${srcdir}/${pkgname}-scangear.desktop" "${pkgdir}/usr/share/applications/${pkgname}-scangear.desktop"
  install -vDm 644 "${srcdir}/${pkgname}-scangear-icon.png" "${pkgdir}/usr/share/pixmaps/${pkgname}-scangear-icon.png"
}
