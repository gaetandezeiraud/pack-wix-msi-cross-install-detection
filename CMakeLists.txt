cmake_minimum_required(VERSION 3.28)

# Enable Hot Reload for MSVC compilers if supported.
if(POLICY CMP0141)
  cmake_policy(SET CMP0141 NEW)
  set(CMAKE_MSVC_DEBUG_INFORMATION_FORMAT "$<IF:$<AND:$<C_COMPILER_ID:MSVC>,$<CXX_COMPILER_ID:MSVC>>,$<$<CONFIG:Debug,RelWithDebInfo>:EditAndContinue>,$<$<CONFIG:Debug,RelWithDebInfo>:ProgramDatabase>>")
endif()

project("AppMsiTest")

# Include sub-projects
add_subdirectory("AppMsiTest")

# Install rules
install(TARGETS AppMsiTest DESTINATION bin COMPONENT App)
install(DIRECTORY "${CMAKE_SOURCE_DIR}/Data/FolderA/" DESTINATION Data/FolderA COMPONENT FolderA OPTIONAL)
install(DIRECTORY "${CMAKE_SOURCE_DIR}/Data/FolderB/" DESTINATION Data/FolderB COMPONENT FolderB OPTIONAL)

# CPack configuration
set(CPACK_GENERATOR "WIX")
set(CPACK_PACKAGE_NAME "AppMsiTest")
set(CPACK_PACKAGE_VERSION "1.0.0")
set(CPACK_PACKAGE_INSTALL_DIRECTORY "AppMsiTest")
set(CPACK_WIX_UPGRADE_GUID "12345678-1234-1234-1234-1234567890AB")
set(CPACK_WIX_ARCHITECTURE "x64")
set(CPACK_WIX_SKIP_PROGRAM_FOLDER TRUE)
set(CPACK_COMPONENT_APP_REQUIRED TRUE)
#set(CPACK_COMPONENTS_ALL "App")
set(CPACK_WIX_UI_REF "WixUI_Minimal")

# MSI-specific settings
if(MSI_TYPE STREQUAL "A")
  set(CPACK_WIX_PRODUCT_GUID "11111111-1111-1111-1111-111111111111")
  set(CPACK_PACKAGE_FILE_NAME "AppMsiTest-FolderA")
  set(CPACK_WIX_PATCH_FILE "${CMAKE_SOURCE_DIR}/patch_a.xml")
else()
  set(CPACK_WIX_PRODUCT_GUID "22222222-2222-2222-2222-222222222222")
  set(CPACK_PACKAGE_FILE_NAME "AppMsiTest-FolderB")
  set(CPACK_WIX_PATCH_FILE "${CMAKE_SOURCE_DIR}/patch_b.xml")
endif()

include(CPackComponent)

cpack_add_component(App DESCRIPTION "Core application" REQUIRED)
if(MSI_TYPE STREQUAL "A")
	cpack_add_component(FolderA DESCRIPTION "Folder A data")
	cpack_add_component(FolderB DESCRIPTION "Folder B data" DISABLED)
else()
	cpack_add_component(FolderA DESCRIPTION "Folder A data" DISABLED)
	cpack_add_component(FolderB DESCRIPTION "Folder B data")
endif()


include(CPack)