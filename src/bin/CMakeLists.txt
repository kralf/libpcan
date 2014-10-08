remake_find_package(popt CONFIG)

remake_include()

remake_add_executable(receive receivetest.c common.c LINK pcan)
remake_add_executable(transmit transmitest.cpp common.c parser.cpp LINK pcan)
remake_add_executable(bitrate bitratetest.c common.c LINK pcan)
remake_add_executable(filter filtertest.cpp common.c LINK pcan)
remake_add_executable(settings pcan-settings.c LINK pcan ${POPT_LIBRARIES})
