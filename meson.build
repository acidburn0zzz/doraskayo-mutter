project('mutter', 'c',
  version: '3.30.1',
  meson_version: '>= 0.46.0',
  license: 'GPLv2+'
)

mutter_plugin_api_version = '3'

libmutter_api_version = '3'

# generic version requirements
glib_req = '>= 2.53.2'
gi_req = '>= 0.9.5'
gtk3_req = '>= 3.19.8'
gdk_pixbuf_req = '>= 2.0'
uprof_req = '>= 0.3'
pango_req = '>= 1.2.0'
cairo_req = '>= 1.10.0'
pangocairo_req = '>= 1.20'
gsettings_desktop_schemas_req = '>= 3.21.4'
json_glib_req = '>= 0.12.0'
upower_glib_req = '>= 0.99.0'
xcomposite_req = '>= 0.4'
xkbcommon_req = '>= 0.4.3'
xfixes_req = '>= 3'
xi_req = '>= 1.6.99.1'
xrandr_req = '>= 1.5.0'
libstartup_notification_req = '>= 0.7'
libcanberra_gtk3_req = '>= 0.26'
libwacom_req = '>= 0.13'
atk_req = '>= 2.5.3'

# optional version requirements
udev_req = '>= 228'
gudev_req = '>= 232'

# wayland version requirements
wayland_server_req = '>= 1.13.0'
wayland_protocols_req = '>= 1.16'

# native backend version requirements
libinput_req = '>= 1.4'
gbm_req = '>= 10.3'

# screen cast version requirements
libpipewire_req = '>= 0.2.2'

gnome = import('gnome')
pkg = import('pkgconfig')
i18n  = import('i18n')
cc = meson.get_compiler('c')

prefix = get_option('prefix')

bindir = join_paths(prefix, get_option('bindir'))
datadir = join_paths(prefix, get_option('datadir'))
libdir = join_paths(prefix, get_option('libdir'))
libexecdir = join_paths(prefix, get_option('libexecdir'))
includedir = join_paths(prefix, get_option('includedir'))
sysconfdir = get_option('sysconfdir')

pkgname = '@0@-@1@'.format(meson.project_name(), libmutter_api_version)

pkgdatadir = join_paths(datadir, pkgname)
pkglibdir = join_paths(libdir, pkgname)
pkgincludedir = join_paths(includedir, pkgname)

gettext_package = meson.project_name()
localedir = join_paths(datadir, 'locale')

top_srcdir = meson.current_source_dir()
builddir = meson.current_build_dir()

m_dep = cc.find_library('m', required: true)
x11_dep = dependency('x11')
gtk3_dep = dependency('gtk+-3.0', version: gtk3_req)
gdk_pixbuf_dep = dependency('gdk-pixbuf-2.0')
pango_dep = dependency('pango', version: pango_req)
cairo_dep = dependency('cairo', version: cairo_req)
cairo_gobject_dep = dependency('cairo-gobject', version: cairo_req)
pangocairo_dep = dependency('pangocairo', version: pangocairo_req)
gsettings_desktop_schemas_dep = dependency('gsettings-desktop-schemas',
                                           version: gsettings_desktop_schemas_req)
glib_dep = dependency('glib-2.0', version: glib_req)
gio_dep = dependency('gio-unix-2.0', version: glib_req)
gio_unix_dep = dependency('gio-unix-2.0', version: glib_req)
gobject_dep = dependency('gobject-2.0', version: glib_req)
gthread_dep = dependency('gobject-2.0', version: glib_req)
gmodule_no_export_dep = dependency('gmodule-no-export-2.0', version: glib_req)
gnome_settings_daemon_dep = dependency('gnome-settings-daemon')
json_glib_dep = dependency('json-glib-1.0', version: json_glib_req)
gnome_desktop_dep = dependency('gnome-desktop-3.0')
xcomposite_dep = dependency('xcomposite', version: xcomposite_req)
xcursor_dep = dependency('xcursor')
xdamage_dep = dependency('xdamage')
xext_dep = dependency('xext')
xfixes_dep = dependency('xfixes', version: xfixes_req)
xi_dep = dependency('xi', version: xi_req)
xtst_dep = dependency('xtst')
xkbfile_dep = dependency('xkbfile')
xkeyboard_config_dep = dependency('xkeyboard-config')
xkbcommon_dep = dependency('xkbcommon', version: xkbcommon_req)
xkbcommon_x11_dep = dependency('xkbcommon-x11')
xrender_dep = dependency('xrender')
x11_xcb_dep = dependency('x11-xcb')
xrandr_dep = dependency('xrandr', version: xrandr_req)
xcb_randr_dep = dependency('xcb-randr')
xcb_res_dep = dependency('xcb-res')
xinerama_dep = dependency('xinerama')
ice_dep = dependency('ice')
atk_dep = dependency('atk', version: atk_req)

# For now always require X11 support
have_x11 = true

have_gl = get_option('opengl')
if have_gl
  gl_dep = dependency('gl')
  gl_libname = get_option('opengl_libname')
endif

have_egl = get_option('egl')
if have_egl
  egl_dep = dependency('egl')
endif

have_glx = get_option('glx')
if have_glx
  if not have_gl
    error('GLX support requires OpenGL to be enabled')
  endif
endif

have_egl_xlib = have_egl and have_x11

have_gles2 = get_option('gles2')
if have_gles2
  gles2_dep = dependency('glesv2')
  gles2_libname = get_option('gles2_libname')

  if not have_egl
    error('GLESv2 support requires EGL to be enabled')
  endif
endif

have_wayland = get_option('wayland')
if have_wayland
  wayland_server_dep = dependency('wayland-server', version: wayland_server_req)
  wayland_protocols_dep = dependency('wayland-protocols',
                                     version: wayland_protocols_req)
  wayland_egl_dep = dependency('wayland-egl')

  if not have_egl
    error('Wayland support requires EGL to be enabled')
  endif
endif

have_libgudev = get_option('udev')
if have_libgudev
  libudev_dep = dependency('libudev', version: udev_req)
  gudev_dep = dependency('gudev-1.0', version: gudev_req)
endif

have_native_backend = get_option('native_backend')
if have_native_backend
  libdrm_dep = dependency('libdrm')
  libgbm_dep = dependency('gbm', version: gbm_req)
  libinput_dep = dependency('libinput', version: libinput_req)

  libsystemd_dep = dependency('libsystemd')
  if libsystemd_dep.found()
    logind_provider_dep = libsystemd_dep
  else
    logind_provider_dep = dependency('libelogind')
  endif

  if not have_egl
    error('The native backend requires EGL to be enabled')
  endif

  if not have_gles2
    error('The native backend requires GLESv2 to be enabled')
  endif

  if not have_libgudev
    error('The native backend requires udev to be enabled')
  endif
endif

have_egl_device = get_option('egl_device')

have_wayland_eglstream = get_option('wayland_eglstream')
if have_wayland_eglstream
  wayland_eglstream_protocols_dep = dependency('wayland-eglstream-protocols')
  dl_dep = cc.find_library('dl', required: true)

  if not have_wayland
    error('Wayland EGLStream support requires Wayland to be enabled')
  endif
endif

have_sm = get_option('sm')
if have_sm
  sm_dep = dependency('sm')
endif

have_libcanberra = get_option('libcanberra')
if have_libcanberra
  libcanberra_gtk3_dep = dependency('libcanberra-gtk3', version: libcanberra_gtk3_req)
endif

have_libwacom = get_option('libwacom')
if have_libwacom
  libwacom_dep = dependency('libwacom', version: libwacom_req)
endif

have_pango_ft2 = get_option('pango_ft2')
if have_pango_ft2
  pangoft2_dep = dependency('pangoft2')
endif

have_startup_notification = get_option('startup_notification')
if have_startup_notification
  libstartup_notification_dep = dependency('libstartup-notification-1.0',
                                           version: libstartup_notification_req)
endif

have_remote_desktop = get_option('remote_desktop')
if have_remote_desktop
  libpipewire_dep = dependency('libpipewire-0.2', version: libpipewire_req)
endif

have_introspection = get_option('introspection')
if have_introspection
  gobject_introspection_dep = dependency('gobject-introspection-1.0')
endif

have_cogl_tests = get_option('cogl_tests')
have_clutter_tests = get_option('clutter_tests')

have_tests = get_option('tests')
if have_tests
  if not have_wayland
    error('Tests require Wayland to be enabled')
  endif
endif

required_functions = [
  'ffs',
  'clz',
]
foreach function : required_functions
  if not cc.has_function(function)
    error('Required function ' + function + ' missing')
  endif
endforeach

add_project_arguments('-D_GNU_SOURCE', language: 'c')

debug_c_args = []
buildtype = get_option('buildtype')
if buildtype.startswith('debug')
  debug_c_args += '-DG_ENABLE_DEBUG'
endif
add_project_arguments(debug_c_args, language: 'c')

cc.compiles('void main (void) { __builtin_ffsl (0); __builtin_popcountl (0); }')

cdata = configuration_data()
cdata.set_quoted('GETTEXT_PACKAGE', gettext_package)
cdata.set_quoted('VERSION', meson.project_version())
cdata.set_quoted('PACKAGE_VERSION', meson.project_version())

cdata.set('HAVE_EGL', have_egl)
cdata.set('HAVE_WAYLAND', have_wayland)
cdata.set('HAVE_NATIVE_BACKEND', have_native_backend)
cdata.set('HAVE_REMOTE_DESKTOP', have_remote_desktop)
cdata.set('HAVE_EGL_DEVICE', have_egl_device)
cdata.set('HAVE_WAYLAND_EGLSTREAM', have_wayland_eglstream)
cdata.set('HAVE_LIBGUDEV', have_libgudev)
cdata.set('HAVE_LIBCANBERRA', have_libcanberra)
cdata.set('HAVE_LIBWACOM', have_libwacom)
cdata.set('HAVE_SM', have_sm)
cdata.set('HAVE_STARTUP_NOTIFICATION', have_startup_notification)
cdata.set('HAVE_INTROSPECTION', have_introspection)

xkb_base = xkeyboard_config_dep.get_pkgconfig_variable('xkb_base')
cdata.set_quoted('XKB_BASE', xkb_base)

xwayland_path = get_option('xwayland_path')
if xwayland_path == ''
  xwayland_path = find_program('Xwayland').path()
endif
cdata.set_quoted('XWAYLAND_PATH', xwayland_path)

xwayland_grab_default_access_rules = get_option('xwayland_grab_default_access_rules')
cdata.set_quoted('XWAYLAND_GRAB_DEFAULT_ACCESS_RULES',
                 xwayland_grab_default_access_rules)

cdata.set_quoted('MUTTER_PLUGIN_DIR', join_paths(pkglibdir, 'plugins'))
cdata.set_quoted('MUTTER_LOCALEDIR', localedir)
cdata.set_quoted('MUTTER_LIBEXECDIR', libexecdir)
cdata.set_quoted('MUTTER_PKGDATADIR', pkgdatadir)

config_h = configure_file(
  input: 'config.h.meson',
  output: 'config.h',
  configuration: cdata
)

top_includepath = include_directories('.')

subdir('cogl')
subdir('clutter')
subdir('data')
subdir('src')
subdir('po')
subdir('doc/man')