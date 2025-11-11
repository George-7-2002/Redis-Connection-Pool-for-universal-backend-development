cmake_minimum_required(VERSION 3.10)

project(RedisPoolProject VERSION 1.0)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

find_package(PkgConfig REQUIRED)

pkg_check_modules(HIREDIS REQUIRED hiredis)

find_package(spdlog REQUIRED)

find_package(Threads REQUIRED)

include_directories(.)

set(LIB_SRCS
    cache_pool.cc
    config_file_reader.cc
    dlog.cc
    util.cc
    lock.cc 
    util_pdu.cc
)

add_library(redis_pool STATIC ${LIB_SRCS})


target_link_libraries(redis_pool PUBLIC
    Threads::Threads
    spdlog::spdlog
    ${HIREDIS_LIBRARIES} # 来自 pkg_check_modules
)

target_include_directories(redis_pool PUBLIC
    .
    ${HIREDIS_INCLUDE_DIRS} # 来自 pkg_check_modules
)

add_executable(redis_pool_test main.cc)

target_link_libraries(redis_pool_test PRIVATE redis_pool)

message(STATUS "Executable target: ${CMAKE_BINARY_DIR}/redis_pool_test")
