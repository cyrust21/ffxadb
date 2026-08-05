cmake_minimum_required(VERSION 3.14)
project("ffx")

set(CMAKE_CXX_STANDARD 17)

add_compile_options(-Werror=format -fdata-sections -ffunction-sections -fno-exceptions -fno-rtti -fno-threadsafe-statics)

# --- BAGIAN PENTING: Mengunduh BoringSSL ---
include(FetchContent)
FetchContent_Declare(
  boringssl
  GIT_REPOSITORY https://boringssl.googlesource.com/boringssl
  GIT_TAG master # Gunakan branch master (selalu ada)
)

# MATIKAN fitur assembly agar tidak butuh bahasa Go & Perl saat kompilasi
set(OPENSSL_NO_ASM ON CACHE BOOL "" FORCE)

FetchContent_MakeAvailable(boringssl)

# --- Kompilasi File Anda ---
add_executable(libffxstarter.so
        starter.cpp misc.cpp selinux.cpp cgroup.cpp)

# Tautkan dengan library 'crypto' bawaan boringssl
target_link_libraries(libffxstarter.so log crypto)

add_library(adb SHARED
        adb_pairing.cpp misc.cpp)

target_link_libraries(adb log crypto)
