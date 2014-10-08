remake_define(NO_RT ON)

remake_include(lib module)

remake_add_directories(lib)
remake_add_directories(
  bin
  COMPONENT utils
)
remake_add_files(
  module/*
  COMPONENT kernel-source
  INSTALL ${CMAKE_INSTALL_PREFIX}/src/${REMAKE_PROJECT_FILENAME}-${REMAKE_PROJECT_FILENAME_VERSION}
)
remake_add_headers(module/pcan.h)
