#cmake_minimum_required(VERSION 3.15)
#project(setupg LANGUAGES CXX)

# Source files
set(SOURCES
    AssemblyInfo.cpp
    exec.cpp
    pinmap.cpp
    portprms.cpp
    setup.cpp
    stdafx.cpp
    app.rc
    com0com.ico
)

# Precompiled header
set(PCH_HEADER stdafx.h)
set(PCH_SOURCE stdafx.cpp)

# Add executable
add_executable(setupg WIN32 ${SOURCES})

# Precompiled headers (CMake 3.16+)
if(CMAKE_VERSION VERSION_GREATER_EQUAL "3.16")
    target_precompile_headers(setupg PRIVATE ${PCH_HEADER})
endif()

# Set preprocessor definitions
target_compile_definitions(setupg PRIVATE WIN32)

# Set output name and subsystem
set_target_properties(setupg PROPERTIES
    OUTPUT_NAME "setupg"
    RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/$<CONFIG>"
    #LINK_FLAGS "/SUBSYSTEM:WINDOWS /ENTRY:main"
)

# Add .NET references (for C++/CLI Windows Forms)
#target_link_libraries(setupg PRIVATE
#    "System.lib"
#    "System.Data.lib"
#    "System.Drawing.lib"
#    "System.Windows.Forms.lib"
#    "System.Xml.lib"
#)

# Include directories
target_include_directories(setupg PRIVATE ${CMAKE_CURRENT_SOURCE_DIR})

#target_link_libraries(setupg PRIVATE
#    "System.dll"
#    "System.Data.dll"
#    "System.Drawing.dll"
#    "System.Windows.Forms.dll"
#    "System.XML.dll"
#)

# Resource file handling (if needed)
# CMake will handle .rc files automatically for MSVC

# Optional: Set debug/release specific options
if(CMAKE_BUILD_TYPE STREQUAL "Debug")
    target_compile_definitions(setupg PRIVATE _DEBUG)
else()
    target_compile_definitions(setupg PRIVATE NDEBUG)
endif()


#target_compile_options(setupg PRIVATE /clr)
#target_compile_options(setupg PRIVATE /fp:precise) # /fp:strict is

set_property(TARGET setupg PROPERTY VS_GLOBAL_ROOTNAMESPACE setupg)
set_property(TARGET setupg PROPERTY VS_GLOBAL_KEYWORD "ManagedCProj")
set_property(TARGET setupg PROPERTY VS_GLOBAL_CLRSupport "true")
set_property(TARGET setupg PROPERTY VS_DOTNET_TARGET_FRAMEWORK_VERSION "v4.0")
set_property(TARGET setupg PROPERTY VS_DOTNET_REFERENCES "System" "System.Data" "System.Drawing" "System.Windows.Forms" "System.Xml")

# Include .NET-related libraries (if needed)
# For example, if you're using C++/CLI for .NET interop:
if(MSVC)
    message("MSVC....")
    #set_target_properties(setupg PROPERTIES
    #    COMMON_LANGUAGE_RUNTIME "clr"
    #)
    set_target_properties(setupg PROPERTIES COMMON_LANGUAGE_RUNTIME "")

    # Remove /RTC and /EH flags from global flags
    string(REPLACE "/EHsc" "" CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}")
    string(REPLACE "/RTC1" "" CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG}")
    target_compile_options(setupg PRIVATE /clr /EHa /W4)
endif()
