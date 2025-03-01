crypto_plugins = [
  'aes',
  'aes_cbc',
  'base64',
  'base91',
  'blowfish',
  'cps2',
  'des',
  'punycode',
  'rc2',
  'rc4',
  'rc6',
  'rol',
  'ror',
  'rot',
  'serpent',
  'xor',
]

rz_crypto_sources = [
  'des.c',
  'crypto.c',
  'p/crypto_aes.c',
  'p/crypto_aes_cbc.c',
  'p/crypto_base64.c',
  'p/crypto_base91.c',
  'p/crypto_blowfish.c',
  'p/crypto_cps2.c',
  'p/crypto_des.c',
  'p/crypto_punycode.c',
  'p/crypto_rc2.c',
  'p/crypto_rc4.c',
  'p/crypto_rc6.c',
  'p/crypto_rol.c',
  'p/crypto_ror.c',
  'p/crypto_rot.c',
  'p/crypto_serpent.c',
  'p/crypto_serpent_algo.c',
  'p/crypto_xor.c',
]

nettle_proj = subproject('nettle')
aes_dep = nettle_proj.get_variable('aes_dep')

rz_crypto = library('rz_crypto', rz_crypto_sources,
  include_directories: [platform_inc],
  dependencies: [rz_util_dep, aes_dep],
  c_args: library_cflags,
  install: true,
  implicit_include_directories: false,
  install_rpath: rpath_lib,
  link_args: library_linkflags,
  soversion: rizin_libversion,
  name_suffix: lib_name_suffix,
  name_prefix: lib_name_prefix,
)

rz_crypto_dep = declare_dependency(link_with: rz_crypto,
                                  include_directories: [platform_inc])
meson.override_dependency('rz_crypto', rz_crypto_dep)

pkgconfig_mod.generate(rz_crypto,
  subdirs: 'librz',
  version: rizin_version,
  name: 'rz_crypto',
  filebase: 'rz_crypto',
  requires: [
    'rz_util'
  ],
  description: 'rizin foundation libraries',
  variables: [
    'plugindir=@0@'.format(rizin_plugins),
    'datdir=@0@'.format(rizin_datdir_rz),
  ],
)

if not is_static_libs_only
  conf = configuration_data()
  conf.set('RZ_VERSION', rizin_version)
  conf.set('RIZIN_MODULE', rz_crypto.name())
  conf.set('RIZIN_MODULE_DEPS', ' '.join(['rz_util']))
  conf.set('PACKAGE_RELATIVE_PATH', cmake_package_relative_path)
  conf.set('INSTALL_INCDIR', rizin_incdir)
  conf.set('INSTALL_LIBDIR', rizin_libdir)
  conf.set('INSTALL_PLUGDIR', rizin_plugins)
  conf.set('rizin_libname', rz_crypto.name())
  cmake_mod.configure_package_config_file(
    name: conf.get('rizin_libname'),
    input: '../RzModulesConfig.cmake.in',
    install_dir: rizin_cmakedir / conf.get('rizin_libname'),
    configuration: conf,
  )
endif
