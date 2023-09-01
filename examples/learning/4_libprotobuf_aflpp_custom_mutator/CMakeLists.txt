# Create the static library for generated protobuf sources
protobuf_generate_cpp(PROTO_SRCS PROTO_HDRS test.proto)
add_library(proto_4 ${PROTO_SRCS})
set_property(TARGET proto_4 PROPERTY COMPILE_FLAGS ${NO_FUZZING_FLAGS})

# Create the AFL++ custom mutator shared library
add_library(libprotobuf_aflpp_custom_mutator SHARED lpm_aflpp_custom_mutator.cc)
target_link_libraries(libprotobuf_aflpp_custom_mutator proto_4
    protobuf-mutator-libfuzzer ${LIB_PROTO_MUTATOR_FUZZER_LIBRARIES})
set_property(TARGET libprotobuf_aflpp_custom_mutator
    PROPERTY COMPILE_FLAGS ${FUZZING_FLAGS})
set_property(TARGET libprotobuf_aflpp_custom_mutator
    PROPERTY LINK_FLAGS ${FUZZING_FLAGS_BINARY})