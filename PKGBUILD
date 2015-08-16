# Maintainer: Charles Bos <charlesbos1 AT gmail>
# Contributor: Xiao-Long Chen <chenxiaolong@cxl.epac.to>
# Contributor: Jan de Groot <jgc@archlinux.org>
# Contributor: Arjan Timmerman <arjan@archlinux.org>
# Contributor: Wael Nasreddine <gandalf@siemens-mobiles.org>
# Contributor: Tor Krill <tor@krill.nu>
# Contributor: Will Rea <sillywilly@gmail.com>

pkgname=network-manager-applet-ubuntu
_ubuntu_rel=0ubuntu4
pkgver=0.9.8.8
pkgrel=2
epoch=1
pkgdesc="GNOME frontends to NetWorkmanager"
arch=('i686' 'x86_64')
url="http://www.gnome.org/projects/NetworkManager/"
license=('GPL')
depends=("networkmanager>=${pkgver}" 'libsecret' 'gtk3' 'libnotify' 'gnome-icon-theme' 'mobile-broadband-provider-info' 'iso-codes' 'libappindicator-bzr')
makedepends=('intltool' 'gnome-bluetooth' 'gobject-introspection' 'modemmanager')
optdepends=('gnome-bluetooth: for PAN/DUN support')
provides=("network-manager-applet=${pkgver}")
conflicts=('network-manager-applet')
options=('!emptydirs')
install=network-manager-applet.install
source=("http://ftp.gnome.org/pub/GNOME/sources/${pkgname%-*}/${pkgver%.*.*}/${pkgname%-*}-${pkgver}.tar.xz"
        "https://launchpad.net/ubuntu/+archive/primary/+files/${pkgname%-*}_${pkgver}-${_ubuntu_rel}.debian.tar.gz"
        '0001-build-don-t-try-to-build-bluetooth-widget-with-newer.patch')
sha512sums=('a43cb00ac60b9401a035aac22a3384625624ca7e3b852106c55c1dc79a6a409d68f482615df6420778cafee0eb38e0088768198de78af52358db48ce9b7222f7'
            'b5062edaf95e33d98a2b77a7ee688215771fa83d71a0a49b8406388ec80e3f7096e018a6c2a50ec9e9f31c70a44d4758ef41167262128e024d7e49892dd83902'
            '37ea0ec9b025f1883964b1d47395b0303154a662de1c5245b95e620dd722b3c76ff348fd4209733f319584a399568aebace01617399d91e126b4f85c57afe747')

prepare() {
  cd "${srcdir}/${pkgname%-*}-${pkgver}"

  patch -p1 -i "${srcdir}/0001-build-don-t-try-to-build-bluetooth-widget-with-newer.patch"

  # Apply Ubuntu's patches
  for i in $(grep -v '#' "${srcdir}/debian/patches/series"); do
    msg "Applying ${i} ..."
    patch -p1 -i "${srcdir}/debian/patches/${i}"
  done

  sed -i 's/-Werror//g' m4/compiler_warnings.m4
}

build() {
  cd "${srcdir}/${pkgname%-*}-${pkgver}"

  autoreconf -vfi

  ./configure \
    --prefix=/usr \
    --sysconfdir=/etc \
    --localstatedir=/var \
    --libexecdir=/usr/lib/networkmanager \
    --disable-static \
    --disable-maintainer-mode \
    --disable-migration \
    --enable-indicator \
    --with-modem-manager-1

  # https://bugzilla.gnome.org/show_bug.cgi?id=655517
  sed -i -e 's/ -shared / -Wl,-O1,--as-needed\0/g' libtool

  make -j1
}

package() {
  cd "${srcdir}/${pkgname%-*}-${pkgver}"
  make DESTDIR="${pkgdir}/" install

  # Install Ubuntu stuff
  install -dm755 "${pkgdir}"/usr/share/icons/hicolor/22x22/apps/
  install -m644 "${srcdir}"/debian/icons/22/nm-device-wired-secure.png \
                "${pkgdir}"/usr/share/icons/hicolor/22x22/apps/
  install -m644 "${srcdir}"/debian/icons/22/nm-signal-00-secure.png \
                "${pkgdir}"/usr/share/icons/hicolor/22x22/apps/
  ln -snf       nm-signal-00.png \
                "${pkgdir}"/usr/share/icons/hicolor/22x22/apps/gsm-3g-none.png
  ln -snf       nm-signal-00-secure.png \
                "${pkgdir}"/usr/share/icons/hicolor/22x22/apps/gsm-3g-none-secure.png
  install -m644 "${srcdir}"/debian/icons/22/nm-signal-25-secure.png \
                "${pkgdir}"/usr/share/icons/hicolor/22x22/apps/
  ln -snf       nm-signal-25.png \
                "${pkgdir}"/usr/share/icons/hicolor/22x22/apps/gsm-3g-low.png
  ln -snf       nm-signal-25-secure.png \
                "${pkgdir}"/usr/share/icons/hicolor/22x22/apps/gsm-3g-low-secure.png
  install -m644 "${srcdir}"/debian/icons/22/nm-signal-50-secure.png \
                "${pkgdir}"/usr/share/icons/hicolor/22x22/apps/
  ln -snf       nm-signal-50.png \
                "${pkgdir}"/usr/share/icons/hicolor/22x22/apps/gsm-3g-medium.png
  ln -snf       nm-signal-50-secure.png \
                "${pkgdir}"/usr/share/icons/hicolor/22x22/apps/gsm-3g-medium-secure.png
  install -m644 "${srcdir}"/debian/icons/22/nm-signal-75-secure.png \
                "${pkgdir}"/usr/share/icons/hicolor/22x22/apps/
  ln -snf       nm-signal-75.png \
                "${pkgdir}"/usr/share/icons/hicolor/22x22/apps/gsm-3g-high.png
  ln -snf       nm-signal-75-secure.png \
                "${pkgdir}"/usr/share/icons/hicolor/22x22/apps/gsm-3g-high-secure.png
  install -m644 "${srcdir}"/debian/icons/22/nm-signal-100-secure.png \
                "${pkgdir}"/usr/share/icons/hicolor/22x22/apps/
  ln -snf       nm-signal-100.png \
                "${pkgdir}"/usr/share/icons/hicolor/22x22/apps/gsm-3g-full.png
  ln -snf       nm-signal-100-secure.png \
                "${pkgdir}"/usr/share/icons/hicolor/22x22/apps/gsm-3g-full-secure.png

  # Use language packs
  rm -r "${pkgdir}/usr/share/locale/"
}
