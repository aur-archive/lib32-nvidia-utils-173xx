# Current Maintainer: Alessandro Sagratini <ale_sagra@hotmail.com>
# Old maintainer: Christofer Bertonha (ghost-linux)

pkgname=lib32-nvidia-utils-173xx
pkgver=173.14.37
pkgrel=1
pkgdesc="x86 NVIDIA drivers utilities and libraries for x86_64 systems."
arch=('x86_64')
url="http://www.nvidia.com/"
license=('custom')
groups=('lib32')
depends=('lib32-libxext' 'lib32-zlib' 'lib32-gcc-libs')
conflicts=('lib32-libgl' 'lib32-ati-fglrx-utils' 'lib32-nvidia-utils' 'lib32-nvidia-utils-beta ')
provides=('lib32-libgl' 'lib32-nvidia-utils')
license=('custom')
_pkgnr=1
source=(http://download.nvidia.com/XFree86/Linux-x86/${pkgver}/NVIDIA-Linux-x86-${pkgver}-pkg${_pkgnr}.run)
options=('docs' '!strip')
md5sums=('fc2bc82c8ddccb42d5414671b16854e2')

build()
{
    _mytempdir=${srcdir}/temp
    
    # delete old files
    cd ${srcdir}
    [ -d "NVIDIA-Linux-x86-${pkgver}-pkg${_pkgnr}" ] && rm -rf NVIDIA-Linux-x86-${pkgver}-pkg${_pkgnr}
    [ -d "${_mytempdir}" ] && rm -rf ${_mytempdir}
    
    # override nvida install routine and do it the long way.
    sh NVIDIA-Linux-x86-${pkgver}-pkg${_pkgnr}.run --extract-only
    cd NVIDIA-Linux-x86-${pkgver}-pkg${_pkgnr}/usr/
    
    mkdir -p ${_mytempdir}
    mkdir -p ${_mytempdir}/usr/{lib,bin,share/applications,share/pixmaps,man/man1}
    mkdir -p ${_mytempdir}/usr/lib/xorg/modules/{extensions,drivers}
    mkdir -p ${_mytempdir}/usr/share/licenses/nvidia/
    
    install lib/{libGLcore,libGL,libnvidia-cfg,libcuda,tls/libnvidia-tls}.so.${pkgver} \
    ${_mytempdir}/usr/lib/ || return 1
    install -m644 share/man/man1/* ${_mytempdir}/usr/man/man1/ || return 1
    rm ${_mytempdir}/usr/man/man1/nvidia-installer.1.gz || return 1
    install X11R6/lib/libXv* ${_mytempdir}/usr/lib/ || return 1
    install -m644 share/applications/nvidia-settings.desktop ${_mytempdir}/usr/share/applications/ || return 1
    # fix nvidia .desktop file
    sed -e 's:__UTILS_PATH__:/usr/bin:' -e 's:__PIXMAP_PATH__:/usr/share/pixmaps:' -i ${_mytempdir}/usr/share/applications/nvidia-settings.desktop
    install -m644 share/pixmaps/nvidia-settings.png ${_mytempdir}/usr/share/pixmaps/ || return 1
    install X11R6/lib/modules/drivers/nvidia_drv.so ${_mytempdir}/usr/lib/xorg/modules/drivers || return 1
    install X11R6/lib/modules/extensions/libglx.so.${pkgver} ${_mytempdir}/usr/lib/xorg/modules/extensions || return 1
    install -m755 bin/nvidia-{settings,xconfig,bug-report.sh} ${_mytempdir}/usr/bin/ || return 1
    
    install -m644 ${srcdir}/NVIDIA-Linux-x86-${pkgver}-pkg${_pkgnr}/LICENSE ${_mytempdir}/usr/share/licenses/nvidia/ || return 1
    ln -s nvidia ${_mytempdir}/usr/share/licenses/nvidia-utils || return 1
    install -D -m644 ${srcdir}/NVIDIA-Linux-x86-${pkgver}-pkg${_pkgnr}/usr/share/doc/README.txt ${_mytempdir}/usr/share/doc/nvidia/README || return 1
    
    find ${_mytempdir}/usr -type d -exec chmod 755 {} \;
    
    cd ${_mytempdir}
    mkdir -p ${pkgdir}/usr/share/licenses/lib32-nvidia-utils-173xx
    mkdir -p ${pkgdir}/usr/lib32/
    cp -dp usr/share/licenses/nvidia/LICENSE ${pkgdir}/usr/share/licenses/lib32-nvidia-utils-173xx/
    cp -dp usr/lib/*.so* ${pkgdir}/usr/lib32
    
    # fix wrong links
    cd ${pkgdir}/usr/lib32/
    ln -sf libGL.so.${pkgver} libGL.so
    ln -sf libGL.so.${pkgver} libGL.so.1
    ln -sf libGLcore.so.${pkgver} libGLcore.so.1
    ln -sf libnvidia-cfg.so.${pkgver} libnvidia-cfg.so.1
    ln -sf libnvidia-tls.so.${pkgver} libnvidia-tls.so.1
    
    }
    