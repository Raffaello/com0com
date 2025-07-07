# Source files
set(SOURCES
    setup.cpp
)

# Output executable
add_executable(setupc ${SOURCES})

# Architecture-specific library path logic
if(CMAKE_SIZEOF_VOID_P EQUAL 8)
else()
    message(FATAL_ERROR "support only x64")
endif()

target_link_libraries(setupc PRIVATE setup)

# Set output directory if needed
# set_target_properties(setupc PROPERTIES RUNTIME_OUTPUT_DIRECTORY "${CMAKE_SOURCE_DIR}/..")
