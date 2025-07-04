cmake_minimum_required(VERSION 3.17)

# Enable C++14
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

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
    Form1.resx
)

set(HEADERS
    exec.h
    Form1.h
    pinmap.h
    portprms.h
    resource.h
)

# Precompiled header
set(PCH_HEADER stdafx.h)
set(PCH_SOURCE stdafx.cpp)


# Compile Form1.resx to SetupApp.Form1.resources
#add_custom_command(
#    OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/SetupApp.Form1.resources
#    COMMAND resgen.exe ${CMAKE_CURRENT_SOURCE_DIR}/Form1.resx ${CMAKE_CURRENT_BINARY_DIR}/SetupApp.Form1.resources
#    DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/Form1.resx
#    COMMENT "Generating SetupApp.Form1.resources from Form1.resx"
#)

# Create a custom target for the resource
#add_custom_target(ManagedResources ALL
#    DEPENDS ${CMAKE_CURRENT_BINARY_DIR}/SetupApp.Form1.resources
#)

# Add executable
add_executable(setupg ${SOURCES} ${HEADERS})
#add_dependencies(setupg ManagedResources)

target_precompile_headers(setupg PRIVATE ${PCH_HEADER})

#set_source_files_properties(Form1.resx PROPERTIES
#    VS_RESOURCE_GENERATOR "ResXFileCodeGenerator"
#    VS_RESOURCE_GENERATOR_OUTPUT "SetupApp.Form1.resources"
#)

# Embed the .resources file into the assembly
#target_sources(setupg PRIVATE ${CMAKE_CURRENT_BINARY_DIR}/SetupApp.Form1.resources)
#set_source_files_properties(${CMAKE_CURRENT_BINARY_DIR}/SetupApp.Form1 PROPERTIES
#    #VS_DEPLOYMENT_CONTENT 0
#    VS_TOOL_OVERRIDE "EmbeddedResource"
#    HEADER_FILE_ONLY FALSE
#    # Logical name must match what ResourceManager expects:
#    # <DefaultNamespace>.<FormName>.resources
#    # If your default namespace is SetupApp, this is correct.
#    VS_RESOURCE_NAME "SetupApp.Form1.resources"
#)

# Include directories
target_include_directories(setupg PRIVATE ${CMAKE_CURRENT_SOURCE_DIR})

# Resource file handling (if needed)
# CMake will handle .rc files automatically for MSVC

# Optional: Set debug/release specific options
if(CMAKE_BUILD_TYPE STREQUAL "Debug")
    target_compile_definitions(setupg PRIVATE _DEBUG)
else()
    target_compile_definitions(setupg PRIVATE NDEBUG)
endif()


set_target_properties(setupg PROPERTIES
    COMMON_LANGUAGE_RUNTIME ""
    VS_CHARSET "Unicode"
    VS_GLOBAL_CLRSupport "Pure"
    VS_GLOBAL_ROOTNAMESPACE SetupApp
    VS_GLOBAL_KEYWORD "ManagedCProj"
    #VS_GLOBAL_CLRSupport "true"
    VS_DOTNET_REFERENCES "System;System.Windows;System.Windows.Forms;System.Drawing;System.Data;System.ComponentModel"
    
    #DOTNET_TARGET_FRAMEWORK "v4.7.2"
    #VS_DOTNET_REFERENCES "System" "System.Data" "System.Drawing" "System.Windows.Forms" "System.Xml"
)

# Include .NET-related libraries (if needed)
# For example, if you're using C++/CLI for .NET interop:
if(MSVC)
    message("MSVC....")

    # Remove /RTC and /EH flags from global flags
    string(REPLACE "/EHsc" "" CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}")
    string(REPLACE "/RTC1" "" CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG}")
    target_compile_options(setupg PRIVATE /clr /EHa /W4)
endif()
