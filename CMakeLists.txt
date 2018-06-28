cmake_minimum_required(VERSION 3.6)
project(ddcode)

SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -g -fpermissive -fvisibility=hidden -Wall -std=c++14" )

ADD_DEFINITIONS(-DHAVE_CONFIG_H)

add_library(crypto_lib STATIC IMPORTED)
set_target_properties(crypto_lib PROPERTIES IMPORTED_LOCATION
        ${CMAKE_CURRENT_SOURCE_DIR}/3rdparty/openssl/lib/libcrypto.a)

add_library(ssl_lib STATIC IMPORTED)
set_target_properties(ssl_lib PROPERTIES IMPORTED_LOCATION
        ${CMAKE_CURRENT_SOURCE_DIR}/3rdparty/openssl/lib/libssl.a)

add_library(db4_lib STATIC IMPORTED)
set_target_properties(db4_lib PROPERTIES IMPORTED_LOCATION
        ${CMAKE_CURRENT_SOURCE_DIR}/3rdparty/db4/lib/libdb.a)
######

set(BITCOIN_SRC
        src/addrman.cpp
        src/arith_uint256.cpp
        src/base58.cpp
        src/blockencodings.cpp
        src/chain.cpp
        src/chainparamsbase.cpp
        src/clientversion.cpp
        src/coins.cpp
        src/fs.cpp
        src/init.cpp
        src/logging.cpp
        src/net.cpp
        src/net_processing.cpp
        src/netaddress.cpp
        src/netbase.cpp
        src/protocol.cpp
        src/pubkey.cpp
        src/random.cpp
        src/torcontrol.cpp
        src/uint256.cpp
        src/util.cpp
        src/utilstrencodings.cpp
        src/utiltime.cpp
        src/validation.cpp
        src/versionbits.cpp
        src/ui_interface.cpp
        src/utilmoneystr.cpp
        src/crypto/chacha20.cpp
        src/crypto/sha512.cpp
        src/index/txindex.cpp
        src/index/base.cpp
        src/leveldb/util/status.cc
        src/policy/policy.cpp
        src/policy/feerate.cpp
        src/rpc/protocol.cpp
        src/rpc/client.cpp
        src/rpc/server.cpp
        src/script/interpreter.cpp
        src/script/script.cpp
        src/script/sigcache.cpp
        src/script/standard.cpp
        src/secp256k1/src/secp256k1.c
        src/support/cleanse.cpp
        src/wallet/init.cpp
  )

add_library(bitcoin ${BITCOIN_SRC})
target_include_directories(bitcoin PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}/src
        ${CMAKE_CURRENT_SOURCE_DIR}/src/leveldb
        ${CMAKE_CURRENT_SOURCE_DIR}/src/secp256k1/include
        ${CMAKE_CURRENT_SOURCE_DIR}/src/univalue/include
        ${CMAKE_CURRENT_SOURCE_DIR}/3rdparty/db4/include
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

#########
set(BITCOIND_SRC
        src/bitcoind.cpp)

add_executable(bitcoind ${BITCOIND_SRC})
target_include_directories(bitcoind PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}/src
        ${CMAKE_CURRENT_SOURCE_DIR}/src/univalue/include
        )
target_link_libraries(bitcoind
        bitcoin
        univalue
        crypto_lib
        ssl_lib
        db4_lib
        event
        event_pthreads
        boost_system
        boost_filesystem
        boost_program_options
        boost_thread
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



