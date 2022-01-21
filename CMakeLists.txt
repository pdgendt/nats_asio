cmake_minimum_required(VERSION 3.12)

project(nats_asio)

option(ENABLE_TESTS "enable tests" OFF)
option(BUILD_NATS_TOOL "build nats tool" OFF)

find_package(Boost REQUIRED)
find_package(OpenSSL REQUIRED)

include_directories(
    "${CMAKE_CURRENT_LIST_DIR}/include"
)

set(LINCLUDE "${CMAKE_CURRENT_LIST_DIR}/include/nats_asio")

set(NATS_ASIO_LIB_SRC
    "${LINCLUDE}/interface.hpp"
    "impl.hpp"
    "impl.cpp"
)

add_library(nats_asio ${NATS_ASIO_LIB_SRC})

if (BUILD_NATS_TOOL)
    add_executable(nats_tool samples/nats_tool.cpp)
    target_link_libraries(nats_tool nats_asio)
endif()

if (ENABLE_TESTS)
    enable_testing()
    find_package(GTest REQUIRED)
    include_directories(${GTEST_INCLUDE_DIRS})
    add_executable(parser_test tests/check1.cpp)
    target_link_libraries(parser_test GTest::GTest GTest::Main)
    add_test(DefaultTestSuite default_test_suite)
endif ()
