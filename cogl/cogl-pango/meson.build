cogl_pango_sources = [
  'cogl-pango-display-list.c',
  'cogl-pango-display-list.h',
  'cogl-pango-fontmap.c',
  'cogl-pango-glyph-cache.c',
  'cogl-pango-glyph-cache.h',
  'cogl-pango-pipeline-cache.c',
  'cogl-pango-pipeline-cache.h',
  'cogl-pango-private.h',
  'cogl-pango-render.c',
]

cogl_pango_public_headers = [
  'cogl-pango.h',
]

cogl_pango_deps = [
  cogl_deps,
  pango_dep,
  pangocairo_dep,
  libmutter_cogl_dep,
]

libmutter_cogl_pango = shared_library('mutter-cogl-pango-' + libmutter_api_version,
  sources: [cogl_pango_sources, cogl_pango_public_headers],
  soversion: '0.0.0',
  c_args: cogl_c_args,
  include_directories: [cogl_includepath, cogl_path_includepath],
  link_depends: 'libmutter-cogl-pango.map',
  dependencies: [cogl_pango_deps],
  install_rpath: pkglibdir,
  install_dir: pkglibdir,
  install: true,
)

libmutter_cogl_pango_dep = declare_dependency(
  link_with: libmutter_cogl_pango,
)

if have_introspection
  libmutter_cogl_pango_gir = gnome.generate_gir(libmutter_cogl_pango,
    sources: cogl_pango_public_headers,
    nsversion: libmutter_api_version,
    namespace: 'CoglPango',
    symbol_prefix: 'cogl_pango',
    header: 'cogl-pango.h',
    includes: [
      libmutter_cogl_gir[0],
      'Pango-1.0',
      'PangoCairo-1.0'
    ],
    dependencies: [
      cogl_deps,
      pango_dep,
      libmutter_cogl_pango_dep,
    ],
    extra_args: [
      '-UCOGL_COMPILATION',
      '-DG_LOG_DOMAIN=\"CoglPango\"',
      '-U_GNU_SOURCE',
    ],
    install_dir_gir: pkglibdir,
    install_dir_typelib: pkglibdir,
    install: true
  )
endif

cogl_pango_includedir = join_paths(cogl_includedir, 'cogl-pango')
install_headers(cogl_pango_public_headers, subdir: cogl_pango_includedir)

pkg.generate(
  name: 'CoglPango',
  filebase: 'mutter-cogl-pango-' + libmutter_api_version,
  description: 'A text rendering for Cogl in mutter',
  libraries: [libmutter_cogl_pango],
  subdirs: join_paths(pkgname, 'cogl'),
  requires: [cogl_pkg_deps, libmutter_cogl_name],
  version: meson.project_version(),
  variables: [
    'apiversion=' + libmutter_api_version,
  ],
)