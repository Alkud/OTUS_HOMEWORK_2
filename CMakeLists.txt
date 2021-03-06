#CmakeList.txt for OTUS Homework #2

#Check CMake version
cmake_minimum_required(VERSION 3.2)

#Set project name, set PROJECT_VERSION_MINOR value equal to TRAVIS_BUILD NUMBER
project(ip_filter VERSION 0.0.$ENV{TRAVIS_BUILD_NUMBER})

set (BOOST_MIN_VERSION "1.54.0")

#Find Boost UTF component, get BOOST_LIBRARIES and BOOST_INCLUDE_DIR values
find_package(Boost COMPONENTS unit_test_framework REQUIRED)

#Process project_version_minor.h to set OTUS_HW_1_VERSION_MINOR value equal to PROJECT_VERSION_MINOR
configure_file(ip_filter_version.h.in ip_filter_version.h)

#Set executable target
add_executable(ip_filter main.cpp)

#Set library target
add_library(ip_address_works ip_address_works.cpp)

#Set test target_link_libraries
add_executable(otus_hw_2_test otus_hw_2_test.cpp)

#Enable tests
enable_testing()
add_test(
NAME test_hw_2
COMMAND otus_hw_2_test)

#Set properties and compile options for executable target
set_target_properties(ip_filter
  PROPERTIES CXX_STANDARD 14
             CXX_STANDARD_REQUIRED ON
             COMPILE_OPTIONS -Wpedantic -Wall -Wextra
)

#Set properties for test target
set_target_properties(otus_hw_2_test PROPERTIES
  COMPILE_DEFINITIONS BOOST_TEST_DYN_LINK
  INCLUDE_DIRECTORIES ${Boost_INCLUDE_DIR}
)

#Link
target_link_libraries(ip_filter ip_address_works)
target_link_libraries(otus_hw_2_test
  ${Boost_LIBRARIES}
  ip_address_works
)

install(TARGETS ip_filter RUNTIME DESTINATION bin)

message("${PROJECT_NAME} build ${PROJECT_VERSION}")

set(CPACK_GENERATOR DEB)

set(CPACK_PACKAGE_VERSION_MAJOR "${PROJECT_VERSION_MAJOR}")
set(CPACK_PACKAGE_VERSION_MINOR "${PROJECT_VERSION_MINOR}")
set(CPACK_PACKAGE_VERSION_PATCH "${PROJECT_VERSION_PATCH}")

set(CPACK_PACKAGE_CONTACT alexandr_kudinov@mail.ru)

include(CPack)
