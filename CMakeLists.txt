cmake_minimum_required(VERSION 2.8)
project(rdparse)

include_directories(include)

if (WIN32)
  set(ICUUC "sicuuc")
else()
  set(ICUUC "icuuc")
endif()

set(FLAGS "-g" "-Og" "-Wall" "-Werror" "-pedantic" "-std=c11" "-D_POSIX_C_SOURCE=200809L" "-D_GNU_SOURCE")

file(GLOB SOURCES "src/*.c")
file(GLOB HEADERS "include/rdparse/*.h")

add_library(rdparse SHARED ${SOURCES})
set_target_properties(rdparse PROPERTIES PUBLIC_HEADER "${HEADERS}")
target_link_libraries(rdparse ${ICUUC})
target_compile_options(rdparse PRIVATE ${FLAGS})

find_package(PkgConfig)

pkg_check_modules (LIBICU REQUIRED icu-uc)

install(TARGETS rdparse
  LIBRARY DESTINATION lib
  PUBLIC_HEADER DESTINATION include/rdparse
)
