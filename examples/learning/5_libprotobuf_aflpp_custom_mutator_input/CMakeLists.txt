# Create the static library for generated protobuf sources
protobuf_generate_cpp(PROTO_SRCS PROTO_HDRS test.proto)
add_library(proto_5 ${PROTO_SRCS})
set_property(TARGET proto_5 PROPERTY COMPILE_FLAGS ${NO_FUZZING_FLAGS})

# Create the AFL++ custom mutator shared library
add_library(libprotobuf_aflpp_custom_mutator_input SHARED
    lpm_aflpp_custom_mutator_input.cc)
target_link_libraries(libprotobuf_aflpp_custom_mutator_input proto_5
    protobuf-mutator-libfuzzer ${LIB_PROTO_MUTATOR_FUZZER_LIBRARIES})
set_property(TARGET libprotobuf_aflpp_custom_mutator_input
    PROPERTY COMPILE_FLAGS ${FUZZING_FLAGS})
set_property(TARGET libprotobuf_aflpp_custom_mutator_input
    PROPERTY LINK_FLAGS ${FUZZING_FLAGS_BINARY})

# Create the serializer that generates the initial seed corpus
add_executable(test_proto_serializer test_proto_serializer.cc)
target_link_libraries(test_proto_serializer proto_5 protobuf-mutator-libfuzzer)