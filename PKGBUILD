# Maintainer: Jan Houben <jan@nexttrex.de>
# Contributor: Jesus Alvarez <jeezusjr at gmail dot com>
#
# This PKGBUILD was generated by the archzfs build scripts located at
#
# http://github.com/archzfs/archzfs
#
# ! WARNING !
#
# The archzfs packages are kernel modules, so these PKGBUILDS will only work with the kernel package they target. In this
# case, the archzfs-linux-lts packages will only work with the default linux-lts package! To have a single PKGBUILD target
# many kernels would make for a cluttered PKGBUILD!
#
# If you have a custom kernel, you will need to change things in the PKGBUILDS. If you would like to have AUR or archzfs repo
# packages for your favorite kernel package built using the archzfs build tools, submit a request in the Issue tracker on the
# archzfs github page.
#
pkgbase="zfs-linux-lts-rc"
pkgname=("zfs-linux-lts-rc" "zfs-linux-lts-rc-headers")
_zfsver="2.0.0_rc3"
_kernelver="5.4.69-1"
_extramodules="5.4.69-1-lts"

pkgver="${_zfsver}_$(echo ${_kernelver} | sed s/-/./g)"
pkgrel=1
makedepends=("linux-lts-headers=${_kernelver}")
arch=("x86_64")
url="https://zfsonlinux.org/"
source=("https://github.com/zfsonlinux/zfs/releases/download/zfs-${_zfsver/_/-}/zfs-${_zfsver/_/-}.tar.gz")
sha256sums=("d06ef8baa44a302ff28c6860e4ff1bf454c617198f755dbbce5d46d3bb7bca7b")
license=("CDDL")
depends=("kmod" "zfs-utils-rc=${_zfsver}" "linux-lts=${_kernelver}")

build() {
    cd "${srcdir}/zfs-${_zfsver/_rc*/}"
    ./autogen.sh
    ./configure --prefix=/usr --sysconfdir=/etc --sbindir=/usr/bin --libdir=/usr/lib \
                --datadir=/usr/share --includedir=/usr/include --with-udevdir=/lib/udev \
                --libexecdir=/usr/lib/zfs-${_zfsver} --with-config=kernel \
                --with-linux=/usr/lib/modules/${_extramodules}/build \
                --with-linux-obj=/usr/lib/modules/${_extramodules}/build
    make
}

package_zfs-linux-lts-rc() {
    pkgdesc="Kernel modules for the Zettabyte File System."
    install=zfs.install
    provides=("zfs" "spl")
    groups=("archzfs-linux-lts-rc")
    conflicts=("zfs-dkms" "zfs-dkms-git" "zfs-dkms-rc" "spl-dkms" "spl-dkms-git" 'zfs-linux-lts' 'zfs-linux-lts-git' 'spl-linux-lts')
    cd "${srcdir}/zfs-${_zfsver/_rc*/}"
    make DESTDIR="${pkgdir}" install
    cp -r "${pkgdir}"/{lib,usr}
    rm -r "${pkgdir}"/lib
    # Remove src dir
    rm -r "${pkgdir}"/usr/src
}

package_zfs-linux-lts-rc-headers() {
    pkgdesc="Kernel headers for the Zettabyte File System."
    provides=("zfs-headers" "spl-headers")
    conflicts=("zfs-headers" "zfs-dkms" "zfs-dkms-git" "zfs-dkms-rc" "spl-dkms" "spl-dkms-git" "spl-headers")
    cd "${srcdir}/zfs-${_zfsver/_rc*/}"
    make DESTDIR="${pkgdir}" install
    rm -r "${pkgdir}/lib"
    # Remove reference to ${srcdir}
    sed -i "s+${srcdir}++" ${pkgdir}/usr/src/zfs-*/${_extramodules}/Module.symvers
}
