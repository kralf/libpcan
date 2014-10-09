cmake_minimum_required(VERSION 2.6)

include(ReMake)

remake_project(
  libpcan
  VERSION 7.12
  RELEASE stable
  SUMMARY "Peak CAN interface library by PEAK-System."
  AUTHOR "Klaus Hitschler"
  CONTACT "linux@peak-system.com"
  HOME "http://www.peak-system.com"
  LICENSE "Propietary License"
  PREFIX pcan-
  INSTALL /usr
)

remake_component(
  udev
  CONFIGURATION_DESTINATION /etc/udev
)
remake_add_directories(
  rules
  COMPONENT udev
)
remake_add_directories(data)

remake_pack_deb(
  COMPONENT kernel-source
  DESCRIPTION "kernel module sources"
  DEPENDS dkms
  EXTRA debian/module/*
)
remake_pack_deb(
  COMPONENT udev
  DESCRIPTION "udev rules"
  DEPENDS libpcan-kernel-source udev
  EXTRA debian/rules/*
)
remake_pack_deb(
  DEPENDS libpcan-udev
)
remake_pack_deb(
  COMPONENT utils
  DESCRIPTION "utilities"
  DEPENDS libpcan libpopt0
)
remake_pack_deb(
  COMPONENT dev
  DESCRIPTION "development headers"
  DEPENDS libpcan
)

remake_distribute_deb(
  DISTRIBUTION lucid
  SECTION libs
  UPLOAD ppa:kralf/asl
  DEPENDS dkms remake pkg-config libpopt-dev
  PASS CMAKE_BUILD_TYPE LIBPCAN_GIT_REVISION
)
remake_distribute_deb(
  DISTRIBUTION precise
  SECTION libs
  UPLOAD ppa:kralf/asl
  DEPENDS dkms remake pkg-config libpopt-dev
  PASS CMAKE_BUILD_TYPE LIBPCAN_GIT_REVISION
)
remake_distribute_deb(
  DISTRIBUTION trusty
  SECTION libs
  UPLOAD ppa:kralf/asl
  DEPENDS dkms remake pkg-config libpopt-dev
  PASS CMAKE_BUILD_TYPE LIBPCAN_GIT_REVISION
)
