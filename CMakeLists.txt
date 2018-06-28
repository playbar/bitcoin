cmake_minimum_required(VERSION 3.6)
project(bitcoin)

SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -g -fpermissive -fvisibility=hidden -Wall -std=c++14" )

ADD_DEFINITIONS(-DHAVE_CONFIG_H)

set(BITCOIN_SRC src/base58.cpp)
add_library(bitcoin ${BITCOIN_SRC})
target_include_directories(bitcoin PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}/src
        )

######

set(BITCOIN_CLI_SRC  src/bitcoin-cli.cpp
                     )
add_executable(bitcoin-cli ${BITCOIN_CLI_SRC})
target_include_directories(bitcoin-cli PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}/src
        ${CMAKE_CURRENT_SOURCE_DIR}/src/univalue/include
        )
target_link_libraries(bitcoin-cli bitcoin)

################



