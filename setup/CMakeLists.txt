set(SOURCES
    setup.cpp
    inffile.cpp
    params.cpp
    devutils.cpp
    portnum.cpp
    comdb.cpp
    msg.cpp
    utils.cpp
    setup.rc
)

# Precompiled header
#set(PCH_HEADER precomp.h)
#set(PCH_SOURCE precomp.cpp) # You may need to create precomp.cpp if not present

# Add library target
add_library(setup SHARED ${SOURCES})

# Precompiled headers (CMake 3.16+)
#if(CMAKE_VERSION VERSION_GREATER_EQUAL "3.16")
#    target_precompile_headers(setup PRIVATE ${PCH_HEADER})
#endif()

# DLL entry point
set_target_properties(setup PROPERTIES
    PREFIX "" # No 'lib' prefix
    SUFFIX ".dll"
    OUTPUT_NAME "setup"
    WINDOWS_EXPORT_ALL_SYMBOLS ON
)

# Link libraries (adjust DDK_LIB_PATH as needed)
target_link_libraries(setup
    kernel32
    advapi32
    setupapi
    newdev
    user32
    msports
    shlwapi
)

# Include directories
target_include_directories(setup PRIVATE ${CMAKE_CURRENT_SOURCE_DIR})

# Use MSVCRT if needed (default for MSVC, but you can force it)
#if(MSVC)
    #target_compile_options(setup PRIVATE /MD)
#endif()

