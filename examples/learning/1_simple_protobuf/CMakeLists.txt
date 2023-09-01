# Create the static library for generated protobuf sources
protobuf_generate_cpp(PROTO_SRCS PROTO_HDRS test.proto)
add_library(proto_1 ${PROTO_SRCS})
set_property(TARGET proto_1 PROPERTY COMPILE_FLAGS ${NO_FUZZING_FLAGS})

# Create the executable and link with the required libraries
add_executable(simple_protobuf test_proto.cc)
target_link_libraries(simple_protobuf proto_1 protobuf-mutator-libfuzzer)