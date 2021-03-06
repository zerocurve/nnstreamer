nnstreamer_inc = include_directories('.')
nnstreamer_sources = []
nnstreamer_headers = []

# Dependencies
nnstreamer_base_deps = [
  glib_dep,
  gst_dep,
  gst_base_dep,
  gst_controller_dep,
  gst_video_dep,
  gst_audio_dep,
  libm_dep,
  libdl_dep,
  thread_dep
]

if have_orcc
  nnstreamer_base_deps += [orc_dep]
endif

# Internal dependencies
nnstreamer_internal_deps = []

# Add nnstreamer registerer and common sources
nnst_common_sources = [
  'nnstreamer.c',
  'nnstreamer_conf.c',
  'nnstreamer_subplugin.c',
  'tensor_common.c'
]

foreach s : nnst_common_sources
  nnstreamer_sources += join_paths(meson.current_source_dir(), s)
endforeach

# Common headers to be installed
nnst_common_headers = [
  'tensor_typedef.h',
  'tensor_filter_custom.h',
  'nnstreamer_plugin_api_filter.h',
  'nnstreamer_plugin_api_decoder.h',
  'nnstreamer_plugin_api.h'
]

foreach h : nnst_common_headers
  nnstreamer_headers += join_paths(meson.current_source_dir(), h)
endforeach

# Add plugins
nnst_plugins = [
  'tensor_aggregator',
  'tensor_converter',
  'tensor_decoder',
  'tensor_demux',
  'tensor_merge',
  'tensor_mux',
  'tensor_sink',
  'tensor_source',
  'tensor_split',
  'tensor_transform',
  'tensor_filter',
  'tensor_repo'
]

foreach p : nnst_plugins
  subdir(p)
endforeach

# Build libraries ("both_libraries" are supported from 0.46.)
nnstreamer_shared = shared_library('nnstreamer',
  nnstreamer_sources,
  dependencies: nnstreamer_base_deps,
  include_directories: nnstreamer_inc,
  install: true,
  install_dir: plugins_install_dir
)

nnstreamer_static = static_library('nnstreamer',
  nnstreamer_sources,
  dependencies: nnstreamer_base_deps,
  include_directories: nnstreamer_inc,
  install: true,
  install_dir: nnstreamer_libdir
)

nnstreamer_lib = nnstreamer_shared
if get_option('default_library') == 'static'
  nnstreamer_lib = nnstreamer_static
endif

nnstreamer_dep = declare_dependency(link_with: nnstreamer_lib,
  dependencies: nnstreamer_base_deps,
  include_directories: nnstreamer_inc)

# Install headers into /{includedir}/nnstreamer
install_headers(nnstreamer_headers,
  subdir: 'nnstreamer'
)
