cmake_minimum_required(VERSION 3.6)
project(ddcode)

SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++14" )

ADD_DEFINITIONS(-DHAVE_CONFIG_H)
ADD_DEFINITIONS(-DLEVELDB_PLATFORM_POSIX)
ADD_DEFINITIONS(-DOS_MACOSX)
#ADD_DEFINITIONS(-DUSE_DB4)

add_library(crypto_lib STATIC IMPORTED)
set_target_properties(crypto_lib PROPERTIES IMPORTED_LOCATION
        ${CMAKE_CURRENT_SOURCE_DIR}/3rdparty/openssl/lib/libcrypto.a)

add_library(ssl_lib STATIC IMPORTED)
set_target_properties(ssl_lib PROPERTIES IMPORTED_LOCATION
        ${CMAKE_CURRENT_SOURCE_DIR}/3rdparty/openssl/lib/libssl.a)

add_library(db_lib_a STATIC IMPORTED)
set_target_properties(db_lib_a PROPERTIES IMPORTED_LOCATION
        ${CMAKE_CURRENT_SOURCE_DIR}/db4/lib/libdb-4.8.a)

add_library(db_cxx_lib_a STATIC IMPORTED)
set_target_properties(db_cxx_lib_a PROPERTIES IMPORTED_LOCATION
        ${CMAKE_CURRENT_SOURCE_DIR}/db4/lib/libdb_cxx-4.8.a)

# db
set(DB_LIB_SRC
        db_src/db/db.c
        )

add_library(db_lib ${DB_LIB_SRC})
target_include_directories(db_lib PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}/db_src
        ${CMAKE_CURRENT_SOURCE_DIR}/db_src/build_unix
        )

# db_cxx
set(DB_CXX_LIB_SRC
        #db_src/db/db.c
        db_src/cxx/cxx_dbc.cpp
        )

add_library(db_cxx_lib ${DB_CXX_LIB_SRC})
target_include_directories(db_cxx_lib PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}/db_src
        ${CMAKE_CURRENT_SOURCE_DIR}/db_src/build_unix
        )

# bitcoin

set(BITCOIN_SRC
        src/addrdb.cpp
        src/addrman.cpp
        src/arith_uint256.cpp
        src/bech32.cpp
        src/base58.cpp
        src/blockencodings.cpp
        src/bloom.cpp
        src/chain.cpp
        src/chainparams.cpp
        src/chainparamsbase.cpp
        src/checkpoints.cpp
        src/clientversion.cpp
        src/coins.cpp
        src/compressor.cpp
        src/core_read.cpp
        src/core_write.cpp
        src/dbwrapper.cpp
        src/fs.cpp
        src/hash.cpp
        src/httprpc.cpp
        src/httpserver.cpp
        src/init.cpp
        src/key.cpp
        src/key_io.cpp
        src/keystore.cpp
        src/logging.cpp
        src/merkleblock.cpp
        src/miner.cpp
        src/net.cpp
        src/net_processing.cpp
        src/netaddress.cpp
        src/netbase.cpp
        src/noui.cpp
        src/pow.cpp
        src/protocol.cpp
        src/pubkey.cpp
        src/random.cpp
        src/rest.cpp
        src/scheduler.cpp
        src/threadinterrupt.cpp
        src/timedata.cpp
        src/torcontrol.cpp
        src/txmempool.cpp
        src/txdb.cpp
        src/uint256.cpp
        src/util.cpp
        src/utilstrencodings.cpp
        src/utiltime.cpp
        src/ui_interface.cpp
        src/utilmoneystr.cpp
        src/validation.cpp
        src/validationinterface.cpp
        src/versionbits.cpp
        src/warnings.cpp
        ###########
        src/compat/glibc_sanity.cpp
        src/compat/glibcxx_sanity.cpp
        src/consensus/merkle.cpp
        src/consensus/tx_verify.cpp
        src/crypto/aes.cpp
        src/crypto/chacha20.cpp
        src/crypto/hmac_sha256.cpp
        src/crypto/hmac_sha512.cpp
        src/crypto/ripemd160.cpp
        src/crypto/sha1.cpp
        src/crypto/sha256.cpp
        src/crypto/sha256_sse4.cpp
        src/crypto/sha512.cpp
        src/index/txindex.cpp
        src/index/base.cpp
        src/leveldb/helpers/memenv/memenv.cc
        src/leveldb/util/status.cc
        src/primitives/block.cpp
        src/primitives/transaction.cpp
        src/policy/feerate.cpp
        src/policy/fees.cpp
        src/policy/policy.cpp
        src/policy/rbf.cpp
        src/rpc/blockchain.cpp
        src/rpc/client.cpp
        src/rpc/mining.cpp
        src/rpc/misc.cpp
        src/rpc/net.cpp
        src/rpc/protocol.cpp
        src/rpc/rawtransaction.cpp
        src/rpc/server.cpp
        src/rpc/util.cpp
        src/script/interpreter.cpp
        src/script/ismine.cpp
        src/script/script.cpp
        src/script/script_error.cpp
        src/script/sigcache.cpp
        src/script/sign.cpp
        src/script/standard.cpp
        src/secp256k1/src/secp256k1.c
        src/support/cleanse.cpp
        src/support/lockedpool.cpp
        src/wallet/coinselection.cpp
        src/wallet/crypter.cpp
        src/wallet/db.cpp
        src/wallet/feebumper.cpp
        src/wallet/fees.cpp
        src/wallet/init.cpp
        src/wallet/rpcdump.cpp
        src/wallet/rpcwallet.cpp
        src/wallet/wallet.cpp
        src/wallet/walletdb.cpp
        src/wallet/walletutil.cpp
  )

add_library(bitcoin ${BITCOIN_SRC})
target_include_directories(bitcoin PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}/src
        ${CMAKE_CURRENT_SOURCE_DIR}/src/leveldb
        ${CMAKE_CURRENT_SOURCE_DIR}/src/leveldb/helpers/memenv
        ${CMAKE_CURRENT_SOURCE_DIR}/src/secp256k1/include
        ${CMAKE_CURRENT_SOURCE_DIR}/src/secp256k1
        ${CMAKE_CURRENT_SOURCE_DIR}/src/univalue/include
        #${CMAKE_CURRENT_SOURCE_DIR}/3rdparty/db4/include
        ${CMAKE_CURRENT_SOURCE_DIR}/db_src/build_unix
        )

### univalue
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
#        db_lib
#        db_cxx_lib
        leveldb
        event
        event_pthreads
        boost_system
        boost_filesystem
        boost_program_options
        boost_thread
        boost_chrono
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



