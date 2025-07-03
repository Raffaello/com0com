# Enable CPack
#include(CPack)

# Set the CPack generator to NSIS
set(CPACK_GENERATOR "NSIS")

# Basic package information
#set(CPACK_PACKAGE_NAME "com0com")
#set(CPACK_PACKAGE_VENDOR "Vyacheslav Frolov")
#set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "Null-modem emulator (com0com)")
#set(CPACK_PACKAGE_VERSION "3.0.1") # Set your version here
#set(CPACK_PACKAGE_INSTALL_DIRECTORY "com0com")
#set(CPACK_NSIS_DISPLAY_NAME "Null-modem emulator (com0com)")
#set(CPACK_NSIS_MUI_ICON "${CMAKE_SOURCE_DIR}/icon.ico") # Optional: set your icon

# License file
#set(CPACK_RESOURCE_FILE_LICENSE "${PROJECT_SOURCE_DIR}/license.txt")

# ReadMe file
#set(CPACK_RESOURCE_FILE_README "${PROJECT_SOURCE_DIR}/ReadMe.txt")

# Optionally, specify your custom NSIS script
set(CPACK_NSIS_SCRIPT "${CMAKE_CURRENT_SOURCE_DIR}/install.nsi")

# Add any other CPack/NSIS options as needed

# Define the output file name for the NSIS installer
#set(CPACK_PACKAGE_FILE_NAME "com0com-${CPACK_PACKAGE_VERSION}-setup")

# Set the default install root (e.g., C:\Program Files)
#set(CPACK_NSIS_INSTALL_ROOT "$PROGRAMFILES")

# Add a custom welcome or finish page
#set(CPACK_NSIS_WELCOME_TITLE "Welcome to com0com Setup")
#set(CPACK_NSIS_FINISH_TITLE "com0com Installation Complete")
set(CPACK_NSIS_OUTPUT_FILE ${CMAKE_BINARY_DIR}/${CPACK_PACKAGE_FILE_NAME})
set(CPACK_NSIS_BASE_DIR ${CMAKE_BINARY_DIR}/${CPACK_PACKAGE_FILE_NAME})

# This must be after all CPACK_* variables
include(CPack)