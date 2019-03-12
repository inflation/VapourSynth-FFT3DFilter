cmake_minimum_required(VERSION 3.13)
project(VapourSynth_FFT3DFilter)

set(CMAKE_CXX_STANDARD 14)

find_package(PkgConfig)
pkg_check_modules(FFTW3F fftw3f)

include_directories(${FFTW3F_INCLUDE_DIRS})
link_directories(${FFTW3F_LIBRARY_DIRS})
link_libraries(${FFTW3F_LIBRARIES} fftw3f_threads)

execute_process(COMMAND git rev-list --count HEAD
        WORKING_DIRECTORY ${PROJECT_SOURCE_DIR}
        OUTPUT_VARIABLE git_rev
        OUTPUT_STRIP_TRAILING_WHITESPACE

        COMMAND git rev-parse --short HEAD
        WORKING_DIRECTORY ${PROJECT_SOURCE_DIR}
        OUTPUT_VARIABLE git_hash
        OUTPUT_STRIP_TRAILING_WHITESPACE)

add_compile_definitions(VSFFT3DFILTER_REV="${git_rev}")
add_compile_definitions(VSFFT3DFILTER_GIT_HASH="${git_hash}")

add_library(fft3dfilter SHARED
    FFT3DFilter.cpp
    fft3dfilter_c.cpp
    Plugin.cpp)
