project('fts', 'c', version : '1.2.7')

cc = meson.get_compiler('c')
cdata = configuration_data()
check_headers = [
  'assert.h',
  'dirent.h',
  'dlfcn.h',
  'errno.h',
  'fcntl.h',
  'inttypes.h',
  'stdint.h',
  'stdio.h',
  'stdlib.h',
  'strings.h',
  'string.h',
  'sys/param.h',
  'sys/stat.h',
  'sys/types.h',
  'unistd.h',
]

foreach h : check_headers
  if cc.has_header(h)
    cdata.set('HAVE_' + h.underscorify().to_upper(), 1)
  endif
endforeach

check_functions = [
  ['HAVE_DIRFD', 'dirfd', '#include <dirent.h>'],
]

foreach f : check_functions
  if cc.has_function(f.get(1), prefix : f.get(2))
    cdata.set(f.get(0), 1)
  endif
endforeach

check_defines = [
  ['HAVE_DECL_MAX', 'MAX', '#include <sys/param.h>'],
  ['HAVE_DECL_UINTMAX_MAX', 'UINTMAX_MAX', '#include <limits.h>'],
]

foreach d : check_defines
  if cc.has_define(d.get(1), prefix : d.get(2))
    cdata.set(d.get(0), 1)
  endif
endforeach

check_members = [
  ['HAVE_DIR_DD_FD', 'DIR', 'dd_fd', '#include <sys/types.h> #include <dirent.h>'],
  ['HAVE_DIR_D_FD', 'DIR', 'd_fd', '#include <sys/types.h> #include <dirent.h>'],
]

foreach m : check_members
  if cc.has_member(m.get(1), m.get(2), prefix : m.get(3))
    cdata.set(m.get(0), 1)
  endif
endforeach

configure_file(input : 'config.h.meson',
  output : 'config.h',
  configuration : cdata)

inc = include_directories('.')

install_headers('fts.h')

fts_sources = ['fts.c']

ftslib = static_library(
  'fts',
  fts_sources,
  include_directories : inc,
  install : true
)

pkg_mod = import('pkgconfig')
pkg_mod.generate(
  libraries : ftslib,
  name: 'musl-fts',
  filebase : 'musl-fts',
  description : 'Implementation of fts(3) functions for musl libc'
)

install_man('fts.3')
