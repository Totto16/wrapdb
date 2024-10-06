project(
    'argparse_c',
    'c',
    default_options: {
        'c_std': 'c11',
        'optimization': '3',
        'warning_level': '3',
    },
    version: '1.1.0',
    license: 'MIT',
)

argparse_c_lib = library(
    'argparse_c',
    files('argparse.c'),
    soversion: meson.project_version(),
    version: meson.project_version(),
)

argparse_c_dep = declare_dependency(
    include_directories: include_directories('.'),
    link_with: argparse_c_lib,
    version: meson.project_version(),
)
