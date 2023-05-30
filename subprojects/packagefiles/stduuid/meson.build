
project(
    'stduuid',
    'cpp',
    default_options: ['default_library=static'],
    version: '1.2.3',
    license: 'MIT',
)

stduuid_version = meson.project_version()
stduuid_so_version = (
    stduuid_version.split('.').get(0) + '.' + stduuid_version.split('.').get(1)
)


flags = []
deps = []
inc_dirs = []
override_options = []

install = get_option('install')

cpp = meson.get_compiler('cpp')
is_msvc_or_similar = cpp.get_id() == 'msvc' or cpp.get_id() == 'clang-cl'

# IS this redundant, or is there no other way?
windows_options_prefix = '-'
if is_msvc_or_similar
    windows_options_prefix = '/'
endif

if get_option('system_generator').allowed()
    if host_machine.system() == 'windows'
        # the needed library is already present
        flags += (windows_options_prefix + 'DUUID_SYSTEM_GENERATOR')
    elif host_machine.system() == 'darwin'
        system_generator_dep = dependency(
            'CoreFoundation',
            'corefoundation',
            required: get_option('system_generator'),
        )
        if system_generator_dep.found()
            deps += system_generator_dep
            flags += '-DUUID_SYSTEM_GENERATOR'
        endif
    else

        system_generator_dep = dependency(
            'libuuid',
            'uuid',
            required: get_option('system_generator'),
        )
        if system_generator_dep.found()
            deps += system_generator_dep
            flags += '-DUUID_SYSTEM_GENERATOR'
        endif
    endif
endif


if get_option('time_generator')
    flags += (windows_options_prefix + 'DUUID_TIME_GENERATOR')
endif

supports_cxx20 = cpp.has_argument(windows_options_prefix + 'std=c++20')


using_cxx20_span = (
    get_option('using_cxx20_span').enabled()
    or (get_option('using_cxx20_span').auto()
    and supports_cxx20)
)

if using_cxx20_span
    override_options += 'cpp_std=c++20'
else
    override_options += 'cpp_std=c++17'
    inc_dirs += include_directories('gsl')
    if install
        install_subdir('gsl', install_dir: 'include')
    endif
endif


inc_dirs += include_directories('include')
header_file = 'include/uuid.h'
if install
    install_headers(header_file)
endif

lib_stduuid = library(
    'stduuid',
    header_file,
    dependencies: deps,
    include_directories: inc_dirs,
    cpp_args: flags,
    install: install,
    override_options: override_options,
    soversion: stduuid_so_version,
    version: stduuid_version,
)


stduuid_dep = declare_dependency(
    include_directories: inc_dirs,
    link_with: lib_stduuid,
    version: stduuid_version,
)


if get_option('tests')
    subdir('test')
endif
