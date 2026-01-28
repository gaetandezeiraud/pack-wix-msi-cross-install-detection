# CMakeLists.txt : CMake project for AppMsiTest
add_executable(AppMsiTest "AppMsiTest.cpp" "AppMsiTest.h")

if(CMAKE_VERSION VERSION_GREATER 3.12)
  set_property(TARGET AppMsiTest PROPERTY CXX_STANDARD 20)
endif()