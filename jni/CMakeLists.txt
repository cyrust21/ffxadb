cmake_minimum_required(VERSION 3.14)
project("ffx")

set(CMAKE_CXX_STANDARD 17)

add_compile_options(-Werror=format -fdata-sections -ffunction-sections -fno-exceptions -fno-rtti -fno-threadsafe-statics)

# --- BAGIAN PENTING: Mengunduh BoringSSL ---
include(FetchContent)
FetchContent_Declare(
  boringssl
  GIT_REPOSITORY https://boringssl.googlesource.com/boringssl
  GIT_TAG 7378d3864aee7cda2f3b9bf19d67a7ffea9f75d9
)

# MATIKAN fitur assembly agar mudah dikompilasi
set(OPENSSL_NO_ASM ON CACHE BOOL "" FORCE)

FetchContent_MakeAvailable(boringssl)

# --- Kompilasi File Anda ---
add_executable(libffxstarter.so
        starter.cpp misc.cpp selinux.cpp cgroup.cpp)

target_link_libraries(libffxstarter.so log crypto)

add_library(adb SHARED
        adb_pairing.cpp misc.cpp)

target_link_libraries(adb log crypto)
