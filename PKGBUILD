# Maintainer: Alonso Rodriguez <alonsorodi20 (at) gmail (dot) com>
# Maintainer: Sven-Hendrik Haase <svenstaro@gmail.com>

pkgbase=nvidia-390xx-settings
pkgname=('nvidia-390xx-settings' 'libxnvctrl-390xx')
pkgver=390.138
pkgrel=1
pkgdesc='Tool for configuring the NVIDIA graphics driver, 390xx legacy branch'
url='https://github.com/NVIDIA/nvidia-settings'
arch=('x86_64')
license=('GPL2')
makedepends=('git' 'inetutils' 'jansson' 'gtk3' 'libxv' 'libvdpau' 'nvidia-390xx-utils' 'libxext')
options=('staticlibs')
source=(nvidia-settings-${pkgver}.tar.gz::https://github.com/NVIDIA/nvidia-settings/archive/${pkgver}.tar.gz
        remove-gtk2-and-libnvctrl.patch
        a7c1f5fce6303a643fadff7d85d59934bd0cf6b6.patch)
sha512sums=('c27c8dbb858f06982e251bd3bf49650d05ca8811d61eb342b43bf00bc595b8d789b38b7ebd76b9ab5786ec5bee84b1cb44ee3f3fb24c40dee1b1b9d535993eb6'
            'cce2dd9ab609ef6fd2bdd54d69954105bdcc74a80b8b604e16872031f15084a6b4b00c0c27fa16f8da301c45571895a2e3eda0fe6e16280df56689e137e2f1df'
            '41734e7ff791b04f989d951e79c94e6c8c0dd06d65640597ae0b1c66e30d54c0d11fa4b91e40a29605ea4ff84401a5e8ffa6ed3c4b0329eb433be43104b4a070')

prepare() {
  cd nvidia-settings-${pkgver}
  patch -p1 -i "${srcdir}/remove-gtk2-and-libnvctrl.patch"
  patch -p1 -i "${srcdir}/a7c1f5fce6303a643fadff7d85d59934bd0cf6b6.patch"
}

build() {
  # Set env variables
  export PREFIX=/usr
  export NV_USE_BUNDLED_LIBJANSSON=0
  
  cd nvidia-settings-${pkgver}
  make
  make -C src/libXNVCtrl
}

package_nvidia-390xx-settings() {
  depends=('jansson' 'gtk3' 'libxv' 'libvdpau' 'nvidia-390xx-utils' 'libxnvctrl-390xx')
  conflicts=('nvidia-settings')
  provides=('nvidia-settings')

  cd nvidia-settings-${pkgver}
  make DESTDIR="${pkgdir}" install

  install -D -m644 doc/nvidia-settings.desktop "${pkgdir}/usr/share/applications/nvidia-settings.desktop"
  install -D -m644 doc/nvidia-settings.png "${pkgdir}/usr/share/pixmaps/nvidia-settings.png"
  sed -e 's:__UTILS_PATH__:/usr/bin:' -e 's:__PIXMAP_PATH__:/usr/share/pixmaps:' -i "${pkgdir}/usr/share/applications/nvidia-settings.desktop"

  #rm "$pkgdir/usr/lib/libnvidia-gtk2.so.$pkgver"
}

package_libxnvctrl-390xx() {
  depends=('libxext')
  conflicts=('libxnvctrl')
  provides=('libxnvctrl')
  pkgdesc='NVIDIA NV-CONTROL X extension, 390xx legacy branch'

  cd nvidia-settings-${pkgver}
  install -Dm 644 doc/{NV-CONTROL-API.txt,FRAMELOCK.txt} -t "${pkgdir}/usr/share/doc/${pkgname}"
  install -Dm 644 samples/{Makefile,README,*.c,*.h,*.mk} -t "${pkgdir}/usr/share/doc/${pkgname}/samples"

  cd src/libXNVCtrl
  install -Dm 644 *.h -t "${pkgdir}/usr/include/NVCtrl"
  install -Dm 644 libXNVCtrl.a -t "${pkgdir}/usr/lib"
  install -Dm 755 libXNVCtrl.so.0.0.0 -t "${pkgdir}/usr/lib"
  ln -s libXNVCtrl.so.0.0.0 "${pkgdir}/usr/lib/libXNVCtrl.so.0"
  ln -s libXNVCtrl.so.0 "${pkgdir}/usr/lib/libXNVCtrl.so"
}

# vim: ts=2 sw=2 et:
