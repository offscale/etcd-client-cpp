etcd-client-cpp
===============

## Development setup

### Dependencies

   - CMake
   - Protocol Buffers
   - C++ compiler

Tested with: CMake 3.13.3; libprotoc 3.6.1; g++ 4.2.1.

### Build

    git clone https://github.com/offscale/etcd-client-cpp --recursive
    mkdir etcd-client-cpp/build && cd $_
    cmake ..
