# Compile

```bash
mkdir build && cd build
cmake .. -DCMAKE_C_COMPILER=clang -DCMAKE_CXX_COMPILER=clang++ -DCMAKE_BUILD_TYPE=Debug -DLIB_PROTO_MUTATOR_DOWNLOAD_PROTOBUF=ON
cd examples/learning
make
```