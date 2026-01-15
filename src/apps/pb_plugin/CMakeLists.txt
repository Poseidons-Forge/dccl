set(PROTOS
  option_extensions.proto
 )

# protobuf 33 uses absl symbols in the compiler API
find_package(absl CONFIG REQUIRED)

# can't use the plugin before we generate it
protobuf_generate_cpp_no_dccl(PROTO_SRCS PROTO_HDRS ${PROTOS})

add_executable(protoc-gen-dccl pb_plugin.cpp ${PROTO_SRCS} ${PROTO_HDRS})
target_link_libraries(protoc-gen-dccl
  ${PROTOBUF_LIBRARIES} ${PROTOBUF_PROTOC_LIBRARY} ${CMAKE_THREAD_LIBS_INIT}
  absl::raw_hash_set
)
install(TARGETS protoc-gen-dccl DESTINATION ${CMAKE_INSTALL_BINDIR})


add_executable(protoc-gen-dccl_cpp pb_plugin.cpp ${PROTO_SRCS} ${PROTO_HDRS})
target_link_libraries(protoc-gen-dccl_cpp
  ${PROTOBUF_LIBRARIES} ${PROTOBUF_PROTOC_LIBRARY} ${CMAKE_THREAD_LIBS_INIT}
  absl::raw_hash_set
)
install(TARGETS protoc-gen-dccl_cpp DESTINATION ${CMAKE_INSTALL_BINDIR})
target_compile_definitions(protoc-gen-dccl_cpp PRIVATE INCLUDE_CPP)
