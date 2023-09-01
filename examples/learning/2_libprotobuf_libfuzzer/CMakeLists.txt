# Create the static library for generated protobuf sources
protobuf_generate_cpp(PROTO_SRCS PROTO_HDRS test.proto)
add_library(proto_2 ${PROTO_SRCS})
set_property(TARGET proto_2 PROPERTY COMPILE_FLAGS ${NO_FUZZING_FLAGS})

# Create the libfuzzer
add_executable(libprotobuf_libfuzzer harness.cc lpm_libfuzz.cc)
target_link_libraries(libprotobuf_libfuzzer proto_2
    protobuf-mutator-libfuzzer ${LIB_PROTO_MUTATOR_FUZZER_LIBRARIES})
set_property(TARGET libprotobuf_libfuzzer
    PROPERTY COMPILE_FLAGS ${FUZZING_FLAGS})
set_property(TARGET libprotobuf_libfuzzer
    PROPERTY LINK_FLAGS ${FUZZING_FLAGS_BINARY})