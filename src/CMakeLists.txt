cmake_minimum_required(VERSION 2.8.12 FATAL_ERROR)

# ==============================================================================
# Check that submodule have been initialized and updated
# ==============================================================================
if(NOT EXISTS ${PROJECT_SOURCE_DIR}/src/third_party/gflags)
    message(FATAL_ERROR
        "\n Submodule gflags is missing, please update your repository:\n"
        "  > git submodule update -i\n")
endif()
if(NOT EXISTS ${PROJECT_SOURCE_DIR}/src/third_party/gtest)
    message(FATAL_ERROR
        "\n Submodule gtest is missing, please update your repository:\n"
        "  > git submodule update -i\n")
endif()

# ================= Set policy ======================================
if(POLICY CMP0042)
    cmake_policy(SET CMP0042 NEW) # MACOSX_RPATH is enabled by default.
endif()

# configure a header file to pass some of the CMake settings to the source code
configure_file (
  "${CMAKE_CURRENT_SOURCE_DIR}/libvot_config.h.in"
  "${CMAKE_CURRENT_SOURCE_DIR}/libvot_config.h"
  )
install (FILES "${CMAKE_CURRENT_SOURCE_DIR}/libvot_config.h" DESTINATION include)

# Specify output bin_path and lib_path
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

# the third parth libs
add_subdirectory(third_party)	# third party libs have to be added first
# utility functions
add_subdirectory(utils)			# utils has to be added before other modules
# gpu related code
add_subdirectory(gpu)
# feature lib
add_subdirectory(feature)
# multiview lib
add_subdirectory(multiview)
# matching lib
add_subdirectory(matching)
# the main vocabulary tree directory
add_subdirectory(vocab_tree)
# the image graph directory
add_subdirectory(image_graph)
# the executable files
add_subdirectory(examples)
# benchmark related files
add_subdirectory(benchmark)
