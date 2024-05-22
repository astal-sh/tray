version_split = meson.project_version().split('.')
api_version = version_split[0] + '.' + version_split[1]
tray_gir = 'AstalTray-' + api_version + '.gir'
tray_typelib = 'AstalTray-' + api_version + '.typelib'
tray_so = 'libastal-tray.so.' + meson.project_version()

deps = [
  dependency('glib-2.0'),
  dependency('gobject-2.0'),
  dependency('gio-2.0'),
  dependency('json-glib-1.0'),
  dependency('gdk-pixbuf-2.0'),
]

sources = files(
  'tray.vala',
  'watcher.vala',
  'trayItem.vala'
)

libtray = library(
  meson.project_name(),
  sources,
  dependencies: deps,
  vala_header: meson.project_name() + '.h',
  vala_vapi: meson.project_name() + '.vapi',
  vala_gir: tray_gir,
  version: meson.project_version(),
  install: true,
  install_dir: [true, true, true, true],
)

import('pkgconfig').generate(
  description: 'libastal-tray',
  libraries: libtray,
  name: meson.project_name(),
  filebase: meson.project_name() + '-' + api_version,
  version: meson.project_version(),
  subdirs: meson.project_name(),
  requires: 'gio-2.0',
  install_dir: get_option('libdir') / 'pkgconfig',
)

if get_option('typelib')
  custom_target(
    tray_typelib,
    command: [
      find_program('g-ir-compiler'),
      '--output', '@OUTPUT@',
      '--shared-library', get_option('prefix') / get_option('libdir') / '@PLAINNAME@',
      meson.current_build_dir() / tray_gir,
    ],
    input: libtray,
    output: tray_typelib,
    depends: libtray,
    install: true,
    install_dir: get_option('libdir') / 'girepository-1.0',
  )
endif
