cmake_minimum_required(VERSION 3.21)

project(com0com VERSION 3.0.1 DESCRIPTION "Null-modem emulator (com0com)")

#TODO: review these lines
#set_property(GLOBAL PROPERTY USER_FOLDERS ON)
#set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /W4 /WX")
#set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} /W4 /WX")

# https://github.com/SergiusTheBest/FindWDK
list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake")

find_package(WDK REQUIRED)


add_subdirectory(sys)
add_subdirectory(setup)
add_subdirectory(setupc)
add_subdirectory(setupg)
add_subdirectory(nsis)
