Cmake_minimum_required(VERSION 3.13.4)

# if CMAKE_SOURCE_DIR equals CMAKE_CURRENT_SOURCE_DIR
# build standalone.
if(CMAKE_SOURCE_DIR STREQUAL CMAKE_CURRENT_SOURCE_DIR)
    project(Tinylang)
    # LLVM package is searched
    find_package(LLVM REQUIRED HINTS "${LLVM_CMAKE_PATH}")
    list(APPEND CMAKE_MODULE_PATH ${LLVM_DIR})
    # addtional CMake modules
    include(AddLLVM)
    include(HandleLLVMOptions)
    # add path of LLVM header files
    include_directories("${LLVM_BINARY_DIR}/include"
                        "${LLVM_INCLUDE_DIR}")
    # add LLVM lib path for linker
    link_directories("${LLVM_LIBRARY_DIR}")

    set(TINYLANG_BUILT_STANDALONE 1)
endif()

# add CMake modules
list(APPEND CMAKE_MODULE_PATH
    "${CMAKE_CURRENT_SOURCE_DIR}/cmake/modules")
# check whether the user is performing an out-of-tree build
if(CMAKE_SOURCE_DIR STREQUAL CMAKE_BINARY_DIR AND NOT MSVC_IDE)
    message(FATAL_ERROR "In-source builds are NOT allowed.")
endif()

set(TINYLANG_VERSION_STRING "0.1")
configure_file(${CMAKE_CURRENT_SOURCE_DIR}/include/tinylang/Basic/Version.inc.in
               ${CMAKE_CURRENT_BINARY_DIR}/include/tinylang/Basic/Version.inc)

# CMake module
include(AddTinylang)

include_directories(BEFORE ${CMAKE_CURRENT_BINARY_DIR}/include
                           ${CMAKE_CURRENT_SOURCE_DIR}/include)

# further directories with CMakeLists.txt
add_subdirectory(lib)
add_subdirectory(tools)

