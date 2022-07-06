# Ref book

- Learn LLVM 12, By Kai Nacke


# How to build

- CC=clang CXX=clang++ cmake .. -G Ninja -DCMAKE_BUILD_TYPE=Release -DLLVM_DIR=/usr/local/lib/cmake/llvm -DCMAKE_INSTALL_PREFIX=install
- ninja
- ninja install

