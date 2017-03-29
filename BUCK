include_defs('//BUCKAROO_DEPS')

def merge_dicts(x, y):
  z = x.copy()
  z.update(y)
  return z

TIF_CONFIG_H = """
#define CCITT_SUPPORT
#define CHECK_JPEG_YCBCR_SUBSAMPLING
#define CXX_SUPPORT
#define DEFAULT_EXTRASAMPLE_AS_ALPHA
#define HAVE_APPLE_OPENGL_FRAMEWORK 0
#define HAVE_ASSERT_H
#define HAVE_DLFCN_H
#define HAVE_FCNTL_H
#define HAVE_FLOOR
#define HAVE_GETOPT
#define HAVE_IEEEFP
#define HAVE_INTTYPES_H
#define HAVE_LFIND
#define HAVE_LIMITS_H
#define HAVE_MEMMOVE
#define HAVE_MEMORY_H
#define HAVE_MEMSET
#define HAVE_MMAP
#define HAVE_POW
#define HAVE_SEARCH_H
#define HAVE_SQRT
#define HAVE_STDINT_H
#define HAVE_STDLIB_H
#define HAVE_STRCASECMP
#define HAVE_STRCHR
#define HAVE_STRINGS_H
#define HAVE_STRING_H
#define HAVE_STRRCHR
#define HAVE_STRSTR
#define HAVE_STRTOL
#define HAVE_STRTOUL
#define HAVE_SYS_STAT_H
#define HAVE_SYS_TIME_H
#define HAVE_SYS_TYPES_H
#define HAVE_UNISTD_H
#define HOST_BIGENDIAN 0
#define HOST_FILLORDER FILLORDER_LSB2MSB
#define LOGLUV_SUPPORT
#define LZW_SUPPORT
#define MDI_SUPPORT 1
#define NEXT_SUPPORT
#define PACKAGE \\"tiff\\"
#define PACKAGE_BUGREPORT \\"tiff@lists.maptools.org\\"
#define PACKAGE_NAME \\"LibTIFF Software\\"
#define PACKAGE_STRING \\"LibTIFF Software 3.8.2\\"
#define PACKAGE_TARNAME \\"tiff\\"
#define PACKAGE_VERSION \\"3.8.2\\"
#define PACKBITS_SUPPORT
#define PIXARLOG_SUPPORT
#define SIZEOF_INT 4
#define SIZEOF_LONG 8
#define STDC_HEADERS
#define STRIPCHOP_DEFAULT TIFF_STRIPCHOP
#define STRIP_SIZE_DEFAULT 8192
#define SUBIFD_SUPPORT
#define THUNDER_SUPPORT
#define TIME_WITH_SYS_TIME
#define VERSION \\"3.8.2\\"
#define X_DISPLAY_MISSING
#define ZIP_SUPPORT
#define const const

#ifndef __cplusplus
# ifndef inline
#  define inline inline
# endif
#endif
"""

TIFFCONF_H = """
#ifndef _TIFFCONF_
#define _TIFFCONF_

#define SIZEOF_INT 4
#define SIZEOF_LONG 8
#define HAVE_IEEEFP
#define HOST_FILLORDER FILLORDER_LSB2MSB
#define CCITT_SUPPORT
#define LOGLUV_SUPPORT
#define LZW_SUPPORT
#define NEXT_SUPPORT
#define PACKBITS_SUPPORT
#define PIXARLOG_SUPPORT
#define THUNDER_SUPPORT
#define ZIP_SUPPORT
#define STRIPCHOP_DEFAULT TIFF_STRIPCHOP
#define SUBIFD_SUPPORT
#define DEFAULT_EXTRASAMPLE_AS_ALPHA
#define CHECK_JPEG_YCBCR_SUBSAMPLING
#define MDI_SUPPORT 1
#define COLORIMETRY_SUPPORT
#define YCBCR_SUPPORT
#define CMYK_SUPPORT
#define ICC_SUPPORT
#define PHOTOSHOP_SUPPORT
#define IPTC_SUPPORT

#endif
"""

genrule(
  name = 'tif_config.h',
  out = 'tif_config.h',
  cmd = 'echo "' + TIF_CONFIG_H + '" > $OUT ',
)

genrule(
  name = 'tiffconf.h',
  out = 'tiffconf.h',
  cmd = 'echo "' + TIFFCONF_H + '" > $OUT ',
)

acorn_sources = [
  'libtiff/tif_acorn.c',
]

atari_sources = [
  'libtiff/tif_atari.c',
]

apple_sources = [
  'libtiff/tif_apple.c',
]

macos_sources = [
  'libtiff/tif_unix.c',
]

linux_sources = [
  'libtiff/tif_unix.c',
]

windows_sources = [
  'libtiff/tif_win3.c',
  'libtiff/tif_win32.c',
]

msdos_sources = [
  'libtiff/tif_msdos.c',
]

platform_sources = acorn_sources + atari_sources + macos_sources + linux_sources + \
                   windows_sources + msdos_sources + apple_sources

cxx_library(
  name = 'libtiff',
  header_namespace = '',
  exported_headers = merge_dicts(subdir_glob([
    ('libtiff', '*.h'),
  ]), {
    'tif_config.h': ':tif_config.h',
    'tiffconf.h': ':tiffconf.h',
  }),
  srcs = glob([
    'libtiff/*.c',
  ],
  excludes = platform_sources),
  platform_srcs = [
    ('default', macos_sources),
    ('^macos.*', macos_sources),
    ('^linux.*', linux_sources),
    ('^windows.*', windows_sources),
  ],
  compiler_flags = [
    '-std=c11',
    '-DHAVE_CONFIG_H',
    '-Dtiffxx_EXPORTS',
  ],
  visibility = [
    'PUBLIC',
  ],
  deps = BUCKAROO_DEPS,
)
