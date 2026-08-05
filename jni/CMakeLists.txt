cmake_minimum_required(VERSION 3.14) # Butuh min 3.14 untuk FetchContent
project("ffx")

set(CMAKE_CXX_STANDARD 17)

add_compile_options(-Werror=format -fdata-sections -ffunction-sections -fno-exceptions -fno-rtti -fno-threadsafe-statics)

# --- BAGIAN PENTING: Mengunduh BoringSSL secara otomatis ---
include(FetchContent)
FetchContent_Declare(
  boringssl
  GIT_REPOSITORY https://boringssl.googlesource.com/boringssl
  # Gunakan commit yang stabil
  GIT_TAG 0cba54b1f67fecdd06c9a7d65ebda314de0360a4 
)
FetchContent_MakeAvailable(boringssl)

# Karena kita tidak menggunakan 'cxx' dari rikka, kita mungkin perlu
# memastikan file header yang dibutuhkan tersedia atau dihapus dari source
# (Jika ada error terkait 'cxx', kita akan tangani nanti)
# find_package(cxx REQUIRED CONFIG) # <- HAPUS ATAU KOMENTARI INI

add_executable(libffxstarter.so
        starter.cpp misc.cpp selinux.cpp cgroup.cpp)

# Tautkan dengan library boringssl yang baru saja diunduh (crypto)
target_link_libraries(libffxstarter.so log crypto)

add_library(adb SHARED
        adb_pairing.cpp misc.cpp)

target_link_libraries(adb log crypto)
