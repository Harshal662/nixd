project( 'nixd'
       , ['c', 'cpp']
       , default_options : ['cpp_std=gnu++20']
       , version: '1.0.0'
       )


config_h = configuration_data()
config_h.set_quoted('NIXD_VERSION', meson.project_version())

configure_file(
  output: 'nixd-config.h',
  configuration: config_h,
)

add_project_arguments([
  '-I' + meson.project_build_root(),
], language: 'cpp')


nix_all = [ dependency('nix-expr')
          , dependency('nix-cmd')
          , dependency('nix-store')
          , dependency('nix-main')
          ]

gtest = dependency('gtest')
gtest_main = dependency('gtest_main')

llvm = dependency('llvm')
boost = dependency('boost')

cpp = meson.get_compiler('cpp')
backtrace = cpp.find_library('backtrace')


subdir('lib')

nixd = executable('nixd', ['tools/nixd/nixd.cpp']
, dependencies: [ backtrace
                , boost
                , llvm
                , nixd_lsp_server
                , nixd_server
                ] + nix_all
, install: true
)

nix_ast_dump = executable('nix-ast-dump', ['tools/nix-ast-dump/nix-ast-dump.cpp']
, dependencies: [ boost
                , nixd_server
                ] + nix_all
)


lit = find_program('lit')

regression_env = environment()

regression_env.append('PATH', meson.current_build_dir())
regression_env.set('MESON_BUILD_ROOT', meson.current_build_dir())

test('regression/nixd', lit, env: regression_env, args: [ '-vv', '../tools/nixd/test' ], depends: [ nixd ] )
test('regression/nix-ast-dump', lit, env: regression_env, args: [ '-vv', '../tools/nix-ast-dump/test' ], depends: [ nix_ast_dump ])
