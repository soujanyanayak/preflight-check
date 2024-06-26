# SPDX-License-Identifier: Apache-2.0
# Copyright (C) 2022 Advanced Micro Devices, Inc.  All rights reserved.
#

# ====================================================================
# This builds the AIE Profile plugin.  It is currently built
# on both Edge and x86 platforms that support AIE.
# ====================================================================

file(GLOB AIE_DEBUG_PLUGIN_FILES
  "${PROFILE_DIR}/plugin/aie_debug/*.h"
  "${PROFILE_DIR}/plugin/aie_debug/*.cpp"
)

if (XDP_MINIMAL_BUILD STREQUAL "yes")
  add_library(xdp_aie_debug_plugin MODULE ${AIE_DEBUG_PLUGIN_FILES})
  add_dependencies(xdp_aie_debug_plugin xdp_core xrt_coreutil)
  include_directories(xdp_aie_debug_plugin PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/transactions)
  target_link_libraries(xdp_aie_debug_plugin PRIVATE xdp_core xrt_coreutil xaiengine)
  target_compile_definitions(xdp_aie_debug_plugin PRIVATE XDP_MINIMAL_BUILD=1 -DXAIE_FEATURE_MSVC)
  target_include_directories(xdp_aie_debug_plugin PRIVATE ${AIERT_DIR}/include)
  set_target_properties(xdp_aie_debug_plugin PROPERTIES VERSION ${XRT_VERSION_STRING} SOVERSION ${XRT_SOVERSION})

  install (TARGETS xdp_aie_debug_plugin
    LIBRARY DESTINATION ${XDP_PLUGIN_INSTALL_DIR}
  )


# Else, on edge-aarch64 don't build at all

endif()
