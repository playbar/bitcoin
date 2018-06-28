cmake_minimum_required(VERSION 3.6)
project(bitcoin)

SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -g -fpermissive -fvisibility=hidden -Wall -std=c++14" )

ADD_DEFINITIONS(-DHAVE_CONFIG_H)

add_library(crypto_lib STATIC IMPORTED)
set_target_properties(crypto_lib PROPERTIES IMPORTED_LOCATION
        ${CMAKE_CURRENT_SOURCE_DIR}/3rdparty/openssl/lib/libcrypto.a)

add_library(ssl_lib STATIC IMPORTED)
set_target_properties(ssl_lib PROPERTIES IMPORTED_LOCATION
        ${CMAKE_CURRENT_SOURCE_DIR}/3rdparty/openssl/lib/libssl.a)

######

set(BITCOIN_SRC
        src/base58.cpp
        src/utilstrencodings.cpp
        src/chainparamsbase.cpp
        src/util.cpp
        src/logging.cpp
        src/fs.cpp
        src/utiltime.cpp
        src/random.cpp
        src/clientversion.cpp
        src/crypto/chacha20.cpp
        src/crypto/sha512.cpp
        src/rpc/protocol.cpp
        src/rpc/client.cpp
        src/support/cleanse.cpp)

add_library(bitcoin ${BITCOIN_SRC})
target_include_directories(bitcoin PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}/src
        ${CMAKE_CURRENT_SOURCE_DIR}/src/univalue/include
        )

###
set(UNIVALUE_SRC
        src/univalue/lib/univalue.cpp
        src/univalue/lib/univalue_get.cpp
        src/univalue/lib/univalue_read.cpp
        src/univalue/lib/univalue_write.cpp)
add_library(univalue ${UNIVALUE_SRC})
target_include_directories(univalue PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}/src/univalue/include
        )

####

set(BITCOIN_CLI_SRC
        src/bitcoin-cli.cpp)

add_executable(bitcoin-cli ${BITCOIN_CLI_SRC})
target_include_directories(bitcoin-cli PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}/src
        ${CMAKE_CURRENT_SOURCE_DIR}/src/univalue/include
        )
target_link_libraries(bitcoin-cli
        bitcoin
        univalue
        crypto_lib
        ssl_lib
        event
        boost_system
        boost_filesystem
        boost_program_options
        boost_thread
        )

################



