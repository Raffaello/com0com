set(DRIVER_INF "${PROJECT_SOURCE_DIR}/com0com.inf")
set(CAT_OUTPUT "${CMAKE_BINARY_DIR}/com0com.cat")

set(SOURCES
    initunlo.c
    adddev.c
    showport.c
    openclos.c
    pnp.c
    power.c
    ioctl.c
    wmi.c
    commprop.c
    read.c
    write.c
    wait.c
    fileinfo.c
    io.c
    pinouts.c
    handflow.c
    startirp.c
    timeout.c
    delay.c
    bufutils.c
    strutils.c
    noise.c
    rand.c
    syslog.c
    trace.c
    tracetbl.c
    c0clog.mc
    #com0com.rc

    ${CMAKE_CURRENT_BINARY_DIR}/c0clog.h
    ${CMAKE_CURRENT_BINARY_DIR}/c0clog.rc
    #${CMAKE_CURRENT_SOURCE_DIR}/c0clog.bin

    #${CAT_OUTPUT}
)

add_custom_command(
    OUTPUT  ${CMAKE_CURRENT_BINARY_DIR}/c0clog.h
            ${CMAKE_CURRENT_BINARY_DIR}/c0clog.rc
            ${CMAKE_CURRENT_BINARY_DIR}/c0clog.bin
    COMMAND mc ${CMAKE_CURRENT_SOURCE_DIR}/c0clog.mc
    DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/c0clog.mc
    COMMENT "Compiling c0clog.mc with mc.exe"
)

wdk_add_driver(
    com0com
    KMDF 1.15 # windows 10 (https://learn.microsoft.com/en-us/windows-hardware/drivers/wdf/kmdf-version-history
    #KMDF 1.3
    WINVER ${WDK_WINVER}
    ${SOURCES}
)

target_include_directories(com0com PRIVATE ${CMAKE_CURRENT_BINARY_DIR})

# Get the directory of the INF file
get_filename_component(DRIVER_INF_DIR "${DRIVER_INF}" DIRECTORY)

#add_custom_command(TARGET com0com POST_BUILD
#    #OUTPUT "${CAT_OUTPUT}"
#    COMMAND inf2cat /driver:"${DRIVER_INF_DIR}" /os:10_X64 /verbose
#    DEPENDS "${DRIVER_INF}"
#    COMMENT "Generating catalog (.cat) file for driver"
#)
