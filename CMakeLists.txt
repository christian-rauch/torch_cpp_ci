cmake_minimum_required(VERSION 3.0 FATAL_ERROR)
project(torch_cpp CXX)

set(TORCH_URL       https://download.pytorch.org/libtorch/cu118/libtorch-cxx11-abi-shared-with-deps-2.1.2%2Bcu118.zip)
set(TORCH_URL_MD5   9a895fc9c942c63154edbc48ae700adb)

include(ExternalProject)
ExternalProject_Add(torch_archive
  URL               ${TORCH_URL}
  URL_MD5           ${TORCH_URL_MD5}
  PREFIX            torch_archive
  CONFIGURE_COMMAND ""
  BUILD_COMMAND     ""
  INSTALL_COMMAND   ""
#   PATCH_COMMAND     find lib -type f -name "*.so*" -o -name "*.a" |  xargs -i strip --strip-all {}
  PATCH_COMMAND
    strip --strip-debug lib/libtorch_global_deps.so lib/libcaffe2_nvrtc.so lib/libtorch_cuda_linalg.so lib/libtorch_python.so lib/libtorch_cpu.so lib/libnvfuser_codegen.so lib/libc10d_cuda_test.so lib/libtorch_cuda.so
)

ExternalProject_Get_Property(torch_archive SOURCE_DIR)

set(TORCH_PATH ${SOURCE_DIR})


# manually install header, libraries and config files

install(DIRECTORY ${TORCH_PATH}/include/
    DESTINATION include
    FILES_MATCHING PATTERN "*.h*"
)

install(DIRECTORY ${TORCH_PATH}/lib/
    DESTINATION lib
)

install(DIRECTORY ${TORCH_PATH}/share/
    DESTINATION share
)
