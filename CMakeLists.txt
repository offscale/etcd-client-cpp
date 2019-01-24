cmake_minimum_required(VERSION 3.13)
project(etcd_client_cpp)

set(CMAKE_CXX_STANDARD 14)

find_package(Protobuf REQUIRED)
include_directories(${Protobuf_INCLUDE_DIRS})
include_directories(${CMAKE_CURRENT_BINARY_DIR})

# gen=$(find -type f -name '*.proto' -exec bash -c 'f="protobuf_generate_cpp("; name="${0##*/}"; name="${name%.*}"; name="${name^^}"; printf "%s\n" "${f}${name}_PROTO_SRC ${name}_PROTO_HDRS $0)"' {} \;);
# echo "$gen"
# gen=$(echo "$gen" | cut -c 23-);
# printf 'set(PROTO_SRCS %s)' "$(while read -r s _; do printf '${%s}\n    ' "$s"; done < <(echo "$gen"))"
# printf 'set(PROTO_HDRS %s)' "$(while read -r s h _; do printf '${%s}\n    ' "$h"; done < <(echo "$gen"))"

protobuf_generate_cpp(RECORD_PROTO_SRC RECORD_PROTO_HDRS ./etcd/wal/walpb/record.proto)
protobuf_generate_cpp(AUTH_PROTO_SRC AUTH_PROTO_HDRS ./etcd/auth/authpb/auth.proto)
protobuf_generate_cpp(LEASE_PROTO_SRC LEASE_PROTO_HDRS ./etcd/lease/leasepb/lease.proto)
protobuf_generate_cpp(KV_PROTO_SRC KV_PROTO_HDRS ./etcd/mvcc/mvccpb/kv.proto)
protobuf_generate_cpp(RAFT_PROTO_SRC RAFT_PROTO_HDRS ./etcd/raft/raftpb/raft.proto)
protobuf_generate_cpp(RPC_PROTO_SRC RPC_PROTO_HDRS ./etcd/functional/rpcpb/rpc.proto)
protobuf_generate_cpp(V3ELECTION_PROTO_SRC V3ELECTION_PROTO_HDRS ./etcd/etcdserver/api/v3election/v3electionpb/v3election.proto)
protobuf_generate_cpp(SNAP_PROTO_SRC SNAP_PROTO_HDRS ./etcd/etcdserver/api/snap/snappb/snap.proto)
protobuf_generate_cpp(V3LOCK_PROTO_SRC V3LOCK_PROTO_HDRS ./etcd/etcdserver/api/v3lock/v3lockpb/v3lock.proto)
protobuf_generate_cpp(RPC_PROTO_SRC RPC_PROTO_HDRS ./etcd/etcdserver/etcdserverpb/rpc.proto)
protobuf_generate_cpp(RAFT_INTERNAL_PROTO_SRC RAFT_INTERNAL_PROTO_HDRS ./etcd/etcdserver/etcdserverpb/raft_internal.proto)
protobuf_generate_cpp(ETCDSERVER_PROTO_SRC ETCDSERVER_PROTO_HDRS ./etcd/etcdserver/etcdserverpb/etcdserver.proto)

set(PROTO_SRCS ${RECORD_PROTO_SRC}
        ${AUTH_PROTO_SRC}
        ${LEASE_PROTO_SRC}
        ${KV_PROTO_SRC}
        ${RAFT_PROTO_SRC}
        ${RPC_PROTO_SRC}
        ${V3ELECTION_PROTO_SRC}
        ${SNAP_PROTO_SRC}
        ${V3LOCK_PROTO_SRC}
        ${RPC_PROTO_SRC}
        ${RAFT_INTERNAL_PROTO_SRC}
        ${ETCDSERVER_PROTO_SRC}
        )

set(PROTO_HDRS ${RECORD_PROTO_HDRS}
        ${AUTH_PROTO_HDRS}
        ${LEASE_PROTO_HDRS}
        ${KV_PROTO_HDRS}
        ${RAFT_PROTO_HDRS}
        ${RPC_PROTO_HDRS}
        ${V3ELECTION_PROTO_HDRS}
        ${SNAP_PROTO_HDRS}
        ${V3LOCK_PROTO_HDRS}
        ${RPC_PROTO_HDRS}
        ${RAFT_INTERNAL_PROTO_HDRS}
        ${ETCDSERVER_PROTO_HDRS}
        )

set_source_files_properties(
        ${AUTH_PROTO_SRCS}
        ${AUTH_PROTO_HDRS}
        PROPERTIES GENERATED 1)

add_executable(etcd_client_cpp main.cpp ${PROTO_SRCS} ${PROTO_HDRS})
target_link_libraries(etcd_client_cpp ${Protobuf_LIBRARIES})
