# Maintainer: bashonly <bashonly@protonmail.com>

pkgname=untrunc-anthwlock-cli-ffmpeg3
pkgver=r308.d72ec32
pkgrel=1
pkgdesc='Restore a truncated mp4/mov. CLI only. (Compiled against ffmpeg 3.3.9)'
arch=('x86_64')
url='https://github.com/anthwlock/untrunc'
license=('GPL2')
depends=(
  'gcc-libs'
  'glibc'
)
makedepends=(
  'git'
  'yasm'
)
provides=('untrunc')
conflicts=(
  'untrunc-anthwlock-cli-git'
  'untrunc-git'
)
_commit=d72ec324fbc29eb00b53c7dafeef09f92308962b
source=(
  "untrunc-anthwlock::git+${url}.git#commit=${_commit}"
  "https://www.ffmpeg.org/releases/ffmpeg-3.3.9.tar.xz"
  "https://www.ffmpeg.org/releases/ffmpeg-3.3.9.tar.xz.asc"
  "ffmpeg-libavcodec-x86-mathops.patch::https://git.ffmpeg.org/gitweb/ffmpeg.git/commitdiff_plain/effadce6c756247ea8bae32dc13bb3e6f464f0eb"
)
noextract=('ffmpeg-3.3.9.tar.xz')
# Import key with: curl https://ffmpeg.org/ffmpeg-devel.asc | gpg --import
validpgpkeys=('FCF986EA15E6E293A5644F10B4322F04D67658D8')  # FFmpeg release signing key <ffmpeg-devel@ffmpeg.org>
sha256sums=(
  'SKIP'
  'ae34f14fffa65a1a59b256737ca9af7bf4e296b7c4320d42512350126ce06c84'
  '29aeae6d7a8748f3a5fb7b284353c70fd6f3432f987fd53e8248526706e5d9fa'
  'f070ac16be68b4d32b1b5b885d146eb36eb508daa928b6f0f78256c3482f9f0e'
)

prepare() {
  cd untrunc-anthwlock
  git clean -dfx
  bsdtar -xf "${srcdir}/ffmpeg-3.3.9.tar.xz"
  cd ffmpeg-3.3.9
  patch -p1 -i "${srcdir}/ffmpeg-libavcodec-x86-mathops.patch"
}

build () {
  cd untrunc-anthwlock/ffmpeg-3.3.9
  # config taken from the anthwlock/untrunc Makefile and needed here to skip some problematic rules
  ./configure --disable-doc --disable-programs \
    --disable-everything --enable-decoders --disable-vdpau --enable-demuxers --enable-protocol=file \
    --disable-avdevice --disable-swresample --disable-swscale --disable-avfilter --disable-postproc \
    --disable-xlib --disable-vaapi --disable-zlib --disable-bzlib --disable-lzma \
    --disable-vda --disable-audiotoolbox --disable-videotoolbox
  cd ..
  make FF_VER=3.3.9
}

package () {
  install -Dm755 untrunc-anthwlock/untrunc -t "${pkgdir}/usr/bin"
}
