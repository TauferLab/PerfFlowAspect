cmake_minimum_required(VERSION 3.1)

project(PerfFlowAspect VERSION "0.1.0")

# Fail if someone tries to config an in-source build.
if("${CMAKE_SOURCE_DIR}" STREQUAL "${CMAKE_BINARY_DIR}")
   message(FATAL_ERROR "In-source builds are not supported. Please remove "
                       "CMakeCache.txt from the 'src' dir and configure an "
                       "out-of-source build in another directory.")
endif()

# Fail if using Clang < 9.0
if("${CMAKE_CXX_COMPILER_ID}" STREQUAL "Clang")
    # require at least Clang 9.0
    if (CMAKE_CXX_COMPILER_VERSION VERSION_LESS 9.0)
        message(FATAL_ERROR "Clang++ version must be at least 9.0!")
    endif()
else()
    message(WARNING "Unsupported CXX compiler: please use Clang >= 9.0")
endif()

# Always use position independent code
set(CMAKE_POSITION_INDEPENDENT_CODE ON)

# Set build type
set(default_build_type "Debug")

if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
  message(STATUS "Using default build type: \"${default_build_type}\"")
  set(CMAKE_BUILD_TYPE "${default_build_type}" CACHE
      STRING "Choose the type of build." FORCE)
  # Set the possible values of build type for cmake-gui
  set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS "Debug" "Release" "MinSizeRel" "RelWithDebInfo")
else()
  message(STATUS "Setting build type to \"${CMAKE_BUILD_TYPE}\"")
endif()

add_subdirectory(parser)
add_subdirectory(runtime)
add_subdirectory(weaver)
add_subdirectory(test)

add_subdirectory(config)

message(STATUS "PerfFlowAspect version: \"${PROJECT_VERSION}\"")
