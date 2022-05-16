#!/bin/bash

# Based on original packaging by Muflone http://www.muflone.com/contacts/english/

# Disable various shellcheck rules that produce false positives in this file.
# Repository rules should be added to the .shellcheckrc file located in the
# repository root directory, see https://github.com/koalaman/shellcheck/wiki
# and https://archiv8.github.io for further information.
# shellcheck disable=SC2034,SC2154

pkgname=ffmpeg-compat-54
pkgver=1.2.12
pkgrel=3
pkgdesc="Compatibility package for ffmpeg to provide versions 54 of libavcodec, libavdevice and libavformat, not anymore provided by the ffmpeg package"
arch=(
  "i686"
  "x86_64"
)
url="http://ffmpeg.org/"
license=(
  "GPL"
)
depends=(
  "gsm"
  "lame"
  "opencore-amr"
  "openjpeg"
  "opus"
  "rtmpdump"
  "libvpx"
  "schroedinger"
  "speex"
  "v4l-utils"
  "xvidcore"
  "libpulse"
  "libx264"
  "libtheora"
  "libbluray"
  "libmodplug"
  "libxfixes"
  "sdl" "jack"
  "libva"
  "libavutil-52"
)
makedepends=(
  "yasm"
  "libass"
)
provides=(
  "libavcodec.so"
  "libavdevice.so"
  "libavformat.so"
)
options=(

  # Add !lto to prevent build failures
  "!lto"
)
source=(
  "http://ffmpeg.org/releases/ffmpeg-${pkgver}.tar.bz2"
  "http://ffmpeg.org/releases/ffmpeg-${pkgver}.tar.bz2.asc"
  "libvpx_VP8E_UPD_ENTROPY.patch"::"https://git.videolan.org/?p=ffmpeg.git;a=commitdiff_plain;h=6540fe04a3f9a11ba7084a49b3ee5fa2fc5b32ab"
  "fix_compilation_with_x264_ge_153_1.patch"::"https://github.com/FFmpeg/FFmpeg/commit/89f704cabab446afc8ba6ecea76714a51b1df32b.patch"
  "fix_compilation_with_x264_ge_153_2.patch"::"https://github.com/FFmpeg/FFmpeg/commit/2a111c99a60fdf4fe5eea2b073901630190c6c93.patch"
  "fix_compilation_with_x264_ge_153_3.patch"::"https://github.com/FFmpeg/FFmpeg/commit/7e60c74329353db28db00552028bc88cd2a52346.patch"
  "fix_compilation_with_x264_ge_153_4.patch"
)
sha512sums=(
  "87d9ab11713b0fd41e3092272dd64f76fe25af8837d9e1c8162df9747f9b7ab6ea26bb201de7820e57de3103e8723019981b5d8c1d5db13ab39131f618c30da2"
  "SKIP"
  "39c2fc65a521c0dbc421a706b81c534e0ffa802f03a8f11a84eefe716ddefca64ca0741c3de3df4d9e1ad9a2658d2daa5bfb28817efbe72f979fc6854951364e"
  "b871b304bfb8d190c442377215170f0be3ef904e6b306dcd3b53500cd539c37598b5510f633a0f78c22f4c33a65a4e741eb4a2fefecb11557043455e3914b8dd"
  "5bbdf452a7871069878a9867ed28a147abe6ac4dba50a1989c367f9d65b9e27cea83ee0d6a8d9623a19d3598e2ff1d20dd6559e20e245bc98e712006b4ad9518"
  "280d495577c12492eb719d1ff4ac81fd83d0a091818c8751dbbd89c1b9a00b17af6e60d1dca844a6ed2d16769298231c5017f1e12c87d476252a79542962e93a"
  "acc3eeac41bb805a3cac84613b56440f534ae04f83e676ef63cc8cbd32f23660ecf342916034dddb175f702c4038507ec744e390b650a78293c0f940137d4664"
)

# validpgpkeys=("FCF986EA15E6E293A5644F10B4322F04D67658D8")

prepare() {

  cd "ffmpeg-${pkgver}"

  patch -p1 -i "../libvpx_VP8E_UPD_ENTROPY.patch"
  # Fixes for libx264 >= 153
  patch -p1 -i "../fix_compilation_with_x264_ge_153_1.patch"
  patch -p1 -i "../fix_compilation_with_x264_ge_153_2.patch"
  patch -p1 -i "../fix_compilation_with_x264_ge_153_3.patch"
  patch -p1 -i "../fix_compilation_with_x264_ge_153_4.patch"
}

build() {

  cd "ffmpeg-${pkgver}"

  ./configure \
    --prefix=/usr \
    --incdir="/usr/include" \
    --shlibdir="/usr/lib" \
    --libdir="/usr/lib" \
    --disable-debug \
    --disable-static \
    --enable-dxva2 \
    --disable-fontconfig \
    --enable-gpl \
    --enable-libass \
    --enable-libbluray \
    --enable-libfreetype \
    --enable-libgsm \
    --enable-libmodplug \
    --enable-libmp3lame \
    --enable-libopencore_amrnb \
    --enable-libopencore_amrwb \
    --enable-libopenjpeg \
    --enable-libopus \
    --enable-libpulse \
    --enable-librtmp \
    --enable-libschroedinger \
    --enable-libspeex \
    --enable-libtheora \
    --enable-libv4l2 \
    --enable-libvorbis \
    --enable-libvpx \
    --enable-libx264 \
    --enable-libxvid \
    --enable-runtime-cpudetect \
    --enable-shared \
    --enable-vdpau \
    --enable-version3 \
    --enable-x11grab \
    --disable-doc \
    --disable-programs \
    --disable-avresample \
    --disable-avfilter \
    --disable-postproc \
    --disable-swresample \
    --disable-swscale

  make
}

package() {

  cd "ffmpeg-${pkgver}"

  make DESTDIR="${pkgdir}" install-libs

  cd "${pkgdir}/usr/lib"

  rm -f *.so libavutil.*
}
