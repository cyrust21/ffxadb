cmake_minimum_required(VERSION 3.22)
project("ffx")

set(CMAKE_CXX_STANDARD 17)
add_compile_options(-Werror=format -fdata-sections -ffunction-sections -fno-exceptions -fno-rtti -fno-threadsafe-statics)

find_package(boringssl REQUIRED CONFIG)
find_package(cxx REQUIRED CONFIG)

add_executable(libffxstarter.so
        starter.cpp misc.cpp selinux.cpp cgroup.cpp)

target_link_libraries(libffxstarter.so log cxx::cxx)

add_library(adb SHARED
        adb_pairing.cpp misc.cpp)

target_link_libraries(adb log boringssl::crypto_static cxx::cxx)
