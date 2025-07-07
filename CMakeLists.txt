cmake_minimum_required(VERSION 3.21)

project(com0com VERSION 4.0.0 DESCRIPTION "Null-modem emulator (com0com)")

# https://github.com/SergiusTheBest/FindWDK
list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake")

find_package(WDK REQUIRED)


add_subdirectory(sys)
add_subdirectory(setup)
add_subdirectory(setupc)
add_subdirectory(setupg)
add_subdirectory(nsis)
