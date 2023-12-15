cmake_minimum_required(VERSION 3.0 FATAL_ERROR)
project(torch_cpp CXX)

set(TORCH_URL       https://download.pytorch.org/libtorch/cpu/libtorch-cxx11-abi-shared-with-deps-2.1.2%2Bcpu.zip)
set(TORCH_URL_MD5   6395886cf4b2a9981306b78e15dad992)

include(ExternalProject)
ExternalProject_Add(torch_archive
  URL               ${TORCH_URL}
  URL_MD5           ${TORCH_URL_MD5}
  PREFIX            torch_archive
  CONFIGURE_COMMAND ""
  BUILD_COMMAND     ""
  INSTALL_COMMAND   ""
  PATCH_COMMAND     find lib -type f -name "*.so*" -o -name "*.a" |  xargs -i strip --strip-all {}
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
