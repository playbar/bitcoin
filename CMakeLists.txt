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
        db_src/btree/bt_compact.c
        db_src/btree/bt_compare.c
        db_src/btree/bt_compress.c
        db_src/btree/bt_conv.c
        db_src/btree/bt_curadj.c
        db_src/btree/bt_cursor.c
        db_src/btree/bt_delete.c
        db_src/btree/bt_method.c
        db_src/btree/bt_open.c
        db_src/btree/bt_put.c
        db_src/btree/bt_rec.c
        db_src/btree/bt_reclaim.c
        db_src/btree/bt_recno.c
        db_src/btree/bt_rsearch.c
        db_src/btree/bt_search.c
        db_src/btree/bt_split.c
        db_src/btree/bt_stat.c
        db_src/btree/bt_upgrade.c
        db_src/btree/bt_verify.c
        db_src/btree/btree_auto.c
        db_src/btree/btree_autop.c
        db_src/common/crypto_stub.c
        db_src/common/db_byteorder.c
        db_src/common/db_compint.c
        db_src/common/db_err.c
        db_src/common/db_getlong.c
        db_src/common/db_idspace.c
        db_src/common/db_log2.c
        db_src/common/db_shash.c
        db_src/common/dbt.c
        db_src/common/mkpath.c
        db_src/common/openflags.c
        db_src/common/os_method.c
        db_src/common/util_arg.c
        db_src/common/util_cache.c
        db_src/common/util_log.c
        db_src/common/util_sig.c
        db_src/common/zerofill.c
        db_src/env/env_alloc.c
        db_src/env/env_config.c
        db_src/env/env_failchk.c
        db_src/env/env_file.c
        db_src/env/env_globals.c
        db_src/env/env_method.c
        db_src/env/env_name.c
        db_src/env/env_open.c
        db_src/env/env_recover.c
        db_src/env/env_region.c
        db_src/env/env_register.c
        db_src/env/env_sig.c
        db_src/env/env_stat.c
        db_src/db/crdel_auto.c
        db_src/db/crdel_autop.c
        db_src/db/crdel_rec.c
        db_src/db/db.c
        db_src/db/db_am.c
        db_src/db/db_auto.c
        db_src/db/db_autop.c
        db_src/db/db_cam.c
        db_src/db/db_cds.c
        db_src/db/db_conv.c
        db_src/db/db_dispatch.c
        db_src/db/db_dup.c
        db_src/db/db_iface.c
        db_src/db/db_join.c
        db_src/db/db_meta.c
        db_src/db/db_method.c
        db_src/db/db_open.c
        db_src/db/db_overflow.c
        db_src/db/db_ovfl_vrfy.c
        db_src/db/db_pr.c
        db_src/db/db_rec.c
        db_src/db/db_reclaim.c
        db_src/db/db_remove.c
        db_src/db/db_rename.c
        db_src/db/db_ret.c
        db_src/db/db_setid.c
        db_src/db/db_setlsn.c
        db_src/db/db_sort_multiple.c
        db_src/db/db_stati.c
        db_src/db/db_truncate.c
        db_src/db/db_upg.c
        db_src/db/db_upg_opd.c
        db_src/db/db_vrfy.c
        db_src/db/db_vrfy_stub.c
        db_src/db/db_vrfyutil.c
        db_src/db/partition.c
        db_src/dbreg/dbreg.c
        db_src/dbreg/dbreg_auto.c
        db_src/dbreg/dbreg_autop.c
        db_src/dbreg/dbreg_rec.c
        db_src/dbreg/dbreg_stat.c
        db_src/dbreg/dbreg_util.c
        db_src/fileops/fileops_auto.c
        db_src/fileops/fileops_autop.c
        db_src/fileops/fop_basic.c
        db_src/fileops/fop_rec.c
        db_src/fileops/fop_util.c
        db_src/hash/hash.c
        db_src/hash/hash_auto.c
        db_src/hash/hash_conv.c
        db_src/hash/hash_dup.c
        db_src/hash/hash_func.c
        db_src/hash/hash_meta.c
        db_src/hash/hash_method.c
        db_src/hash/hash_open.c
        db_src/hash/hash_page.c
        db_src/hash/hash_rec.c
        db_src/hash/hash_reclaim.c
        db_src/hash/hash_stat.c
        db_src/hash/hash_upgrade.c
        db_src/hash/hash_verify.c
        db_src/hmac/hmac.c
        db_src/hmac/sha1.c
        db_src/lock/lock.c
        db_src/lock/lock_deadlock.c
        db_src/lock/lock_failchk.c
        db_src/lock/lock_id.c
        db_src/lock/lock_list.c
        db_src/lock/lock_method.c
        db_src/lock/lock_region.c
        db_src/lock/lock_stat.c
        db_src/lock/lock_stub.c
        db_src/lock/lock_timer.c
        db_src/lock/lock_util.c
        db_src/log/log.c
        db_src/log/log_archive.c
        db_src/log/log_compare.c
        db_src/log/log_debug.c
        db_src/log/log_get.c
        db_src/log/log_method.c
        db_src/log/log_put.c
        db_src/log/log_stat.c
        db_src/mp/mp_alloc.c
        db_src/mp/mp_bh.c
        db_src/mp/mp_fget.c
        db_src/mp/mp_fmethod.c
        db_src/mp/mp_fopen.c
        db_src/mp/mp_fput.c
        db_src/mp/mp_fset.c
        db_src/mp/mp_method.c
        db_src/mp/mp_mvcc.c
        db_src/mp/mp_region.c
        db_src/mp/mp_register.c
        db_src/mp/mp_resize.c
        db_src/mp/mp_stat.c
        db_src/mp/mp_sync.c
        db_src/mp/mp_trickle.c
        db_src/mutex/mut_alloc.c
        db_src/mutex/mut_failchk.c
        db_src/mutex/mut_fcntl.c
        db_src/mutex/mut_method.c
        db_src/mutex/mut_pthread.c
        db_src/mutex/mut_region.c
        db_src/mutex/mut_stat.c
        db_src/mutex/mut_stub.c
        db_src/mutex/mut_tas.c
        db_src/os/os_abort.c
        db_src/os/os_abs.c
        db_src/os/os_addrinfo.c
        db_src/os/os_alloc.c
        db_src/os/os_clock.c
        db_src/os/os_config.c
        db_src/os/os_cpu.c
        db_src/os/os_ctime.c
        db_src/os/os_dir.c
        db_src/os/os_errno.c
        db_src/os/os_fid.c
        db_src/os/os_flock.c
        db_src/os/os_fsync.c
        db_src/os/os_handle.c
        db_src/os/os_getenv.c
        db_src/os/os_map.c
        db_src/os/os_mkdir.c
        db_src/os/os_open.c
        db_src/os/os_pid.c
        db_src/os/os_rename.c
        db_src/os/os_root.c
        db_src/os/os_rpath.c
        db_src/os/os_rw.c
        db_src/os/os_seek.c
        db_src/os/os_stack.c
        db_src/os/os_stat.c
        db_src/os/os_tmpdir.c
        db_src/os/os_truncate.c
        db_src/os/os_uid.c
        db_src/os/os_unlink.c
        db_src/os/os_yield.c
        db_src/qam/qam.c
        db_src/qam/qam_auto.c
        db_src/qam/qam_autop.c
        db_src/qam/qam_conv.c
        db_src/qam/qam_files.c
        db_src/qam/qam_method.c
        db_src/qam/qam_open.c
        db_src/qam/qam_rec.c
        db_src/qam/qam_stat.c
        db_src/qam/qam_upgrade.c
        db_src/qam/qam_verify.c
        db_src/rep/rep_auto.c
        db_src/rep/rep_backup.c
        db_src/rep/rep_elect.c
        db_src/rep/rep_lease.c
        db_src/rep/rep_log.c
        db_src/rep/rep_method.c
        db_src/rep/rep_record.c
        db_src/rep/rep_region.c
        db_src/rep/rep_stat.c
        db_src/rep/rep_util.c
        db_src/rep/rep_verify.c
        db_src/repmgr/repmgr_auto.c
        db_src/repmgr/repmgr_elect.c
        db_src/repmgr/repmgr_method.c
        db_src/repmgr/repmgr_msg.c
        db_src/repmgr/repmgr_net.c
        db_src/repmgr/repmgr_posix.c
        db_src/repmgr/repmgr_queue.c
        db_src/repmgr/repmgr_sel.c
        db_src/repmgr/repmgr_stat.c
        db_src/repmgr/repmgr_util.c
        db_src/txn/txn.c
        db_src/txn/txn_auto.c
        db_src/txn/txn_autop.c
        db_src/txn/txn_chkpt.c
        db_src/txn/txn_failchk.c
        db_src/txn/txn_method.c
        db_src/txn/txn_rec.c
        db_src/txn/txn_recover.c
        db_src/txn/txn_region.c
        db_src/txn/txn_stat.c
        db_src/txn/txn_util.c
        )

add_library(db_lib ${DB_LIB_SRC})
target_include_directories(db_lib PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}/db_src
        ${CMAKE_CURRENT_SOURCE_DIR}/db_src/include
        )

# db_cxx
set(DB_CXX_LIB_SRC
        db_src/cxx/cxx_db.cpp
        db_src/cxx/cxx_dbc.cpp
        db_src/cxx/cxx_dbt.cpp
        db_src/cxx/cxx_env.cpp
        db_src/cxx/cxx_except.cpp
        db_src/cxx/cxx_lock.cpp
        db_src/cxx/cxx_logc.cpp
        db_src/cxx/cxx_mpool.cpp
        db_src/cxx/cxx_multi.cpp
        db_src/cxx/cxx_seq.cpp
        db_src/cxx/cxx_txn.cpp
        db_src/cxx/cxx_dbc.cpp
        )

add_library(db_cxx_lib ${DB_CXX_LIB_SRC})
target_include_directories(db_cxx_lib PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}/db_src
        ${CMAKE_CURRENT_SOURCE_DIR}/db_src/include
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
        ${CMAKE_CURRENT_SOURCE_DIR}/db_src/include
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
        db_lib
        db_cxx_lib
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



