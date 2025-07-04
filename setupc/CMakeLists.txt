# Set warning level
#if(MSVC)
#    add_compile_options(/W4)
#endif()

# Source files
set(SOURCES
    setup.cpp
)

# Output executable
add_executable(setupc ${SOURCES})

# Use MSVCRT (default for /MD, but you can force it)
#if(MSVC)
#    target_compile_options(setupc PRIVATE /MD)
#endif()

# Set entry point if needed (main is default for console apps)
# set_target_properties(setupc PROPERTIES LINK_FLAGS "/ENTRY:main")

# Architecture-specific library path logic
if(CMAKE_SIZEOF_VOID_P EQUAL 8)
    #set(SETUP_LIB_PATH "${CMAKE_BINARY_DIR}/setup.lib")
#elseif(CMAKE_SYSTEM_PROCESSOR MATCHES "^(IA64|ia64)$")
#    set(SETUP_LIB_PATH "${CMAKE_BINARY_DIR}/../../ia64/setup.lib")
else()
    #set(SETUP_LIB_PATH "${CMAKE_BINARY_DIR}/../../i386/setup.lib")
    message(FATAL_ERROR "support only x64")
endif()

target_link_libraries(setupc PRIVATE setup)

# Set output directory if needed
# set_target_properties(setupc PROPERTIES RUNTIME_OUTPUT_DIRECTORY "${CMAKE_SOURCE_DIR}/..")
