1. Installed Ninja
2. Built libtorch from source to enable the use of MPI


Reference this gist to build from source witha tweak to the command to match my environment
https://gist.github.com/lasagnaphil/3e0099816837318e8e8bcab7edcfd5d9

```bash
cmake -DUSE_MKLDNN=ON \
      -DBUILD_CUSTOM_PROTOBUF=OFF \
      -DBUILD_SHARED_LIBS=ON \
      -DUSE_FFMPEG=OFF \
      -DUSE_GFLAGS=ON \
      -DUSE_GLOG=ON \
      -DBUILD_BINARY=OFF \
      -DBUILD_PYTHON=OFF \
      -DBUILD_TEST=OFF \
      -DPYTHON_LIBRARY='' \
      -DUSE_OPENCV=ON \
      -DUSE_SYSTEM_NCCL=ON \
      -DUSE_DISTRIBUTED=ON \
      -DMPI_C_COMPILER=$I_MPI_ROOT/bin/mpiicc \
      -DMPI_CXX_COMPILER=$I_MPI_ROOT/bin/mpiicpc \
      -DCMAKE_BUILD_TYPE=Release \
      -DCMAKE_INSTALL_PREFIX=../pytorch-install \
      -DCMAKE_CXX_FLAGS="-Wno-error=maybe-uninitialized -Wno-error=dangling-pointer -Wno-error=deprecated-declarations -mno-avx512f" \
      -DUSE_FBGEMM=OFF \
      -GNinja \
      ../pytorch


cmake -DUSE_MKLDNN=OFF \
      -DBUILD_CUSTOM_PROTOBUF=OFF \
      -DBUILD_SHARED_LIBS=ON \
      -DUSE_FFMPEG=OFF \
      -DUSE_GFLAGS=ON \
      -DUSE_GLOG=ON \
      -DBUILD_BINARY=OFF \
      -DBUILD_PYTHON=OFF \
      -DBUILD_TEST=OFF \
      -DPYTHON_LIBRARY='' \
      -DUSE_OPENCV=ON \
      -DUSE_SYSTEM_NCCL=ON \
      -DUSE_DISTRIBUTED=ON \
      -DNCCL_VERSION=$(pkg-config nccl --modversion) \
      -DNCCL_VER_CODE=$(sed -n 's/^#define NCCL_VERSION_CODE\s*\(.*\).*/\1/p' /usr/include/nccl.h) \
      -DCUDAHOSTCXX=g++ \
      -DCUDA_HOME=/opt/cuda \
      -DCUDNN_LIB_DIR=/usr/lib \
      -DCUDNN_INCLUDE_DIR=/usr/include \
      -DTORCH_CUDA_ARCH_LIST="5.2;5.3;6.0;6.1;6.2;7.0;7.0+PTX;7.2;7.2+PTX;7.5;7.5+PTX;8.0;8.0+PTX;8.6;8.6+PTX" \
      -DUSE_CUDA=OFF \
      -DUSE_CUDNN=OFF \
      -DBUILD_SHARED_LIBS=ON \
      -DCMAKE_BUILD_TYPE=Release \
      -DCMAKE_INSTALL_PREFIX=../pytorch-install \
      -GNinja \
      ../pytorch



cmake -DUSE_MKLDNN=ON \
      -DBUILD_CUSTOM_PROTOBUF=OFF \
      -DBUILD_SHARED_LIBS=ON \
      -DUSE_FFMPEG=OFF \
      -DUSE_GFLAGS=ON \
      -DUSE_GLOG=ON \
      -DBUILD_BINARY=OFF \
      -DBUILD_PYTHON=OFF \
      -DBUILD_TEST=OFF \
      -DPYTHON_LIBRARY='' \
      -DUSE_OPENCV=ON \
      -DUSE_SYSTEM_NCCL=ON \
      -DUSE_DISTRIBUTED=ON \
      -DCMAKE_BUILD_TYPE=Release \
      -DCMAKE_INSTALL_PREFIX=../pytorch-install \
      -DCMAKE_C_COMPILER=$HOME/local/gcc-14.2.0/bin/gcc \
      -DCMAKE_CXX_COMPILER=$HOME/local/gcc-14.2.0/bin/g++ \
      -DCMAKE_CXX_FLAGS="-Wno-error=maybe-uninitialized -Wno-error=dangling-pointer -Wno-error=deprecated-declarations -mno-avx512f" \
      -DUSE_FBGEMM=OFF \
      -GNinja \
      ../pytorch

cmake -DUSE_MKLDNN=ON \
      -DBUILD_CUSTOM_PROTOBUF=OFF \
      -DBUILD_SHARED_LIBS=ON \
      -DUSE_FFMPEG=OFF \
      -DUSE_GFLAGS=ON \
      -DUSE_GLOG=ON \
      -DBUILD_BINARY=OFF \
      -DBUILD_PYTHON=ON \
      -DBUILD_TEST=OFF \
      -DPYTHON_EXECUTABLE=$HOME/torchenv/bin/python3.11 \
      -DPYTHON_INCLUDE_DIR=$HOME/torchenv/include/python3.11 \
      -DUSE_OPENCV=ON \
      -DUSE_SYSTEM_NCCL=ON \
      -DUSE_DISTRIBUTED=ON \
      -DCMAKE_BUILD_TYPE=Release \
      -DCMAKE_INSTALL_PREFIX=../pytorch-install \
      -DCMAKE_C_COMPILER=$HOME/local/gcc-14.2.0/bin/gcc \
      -DCMAKE_CXX_COMPILER=$HOME/local/gcc-14.2.0/bin/g++ \
      -DCMAKE_CXX_FLAGS="-Wno-error=maybe-uninitialized -Wno-error=dangling-pointer -Wno-error=deprecated-declarations -mno-avx512f" \
      -DUSE_FBGEMM=OFF \
      -GNinja \
      ../pytorch




cmake -DUSE_MKLDNN=ON \
      -DBUILD_CUSTOM_PROTOBUF=OFF \
      -DBUILD_SHARED_LIBS=ON \
      -DUSE_FFMPEG=OFF \
      -DUSE_GFLAGS=ON \
      -DUSE_GLOG=ON \
      -DBUILD_BINARY=OFF \
      -DBUILD_PYTHON=ON \
      -DBUILD_TEST=OFF \
      -DUSE_OPENCV=ON \
      -DUSE_SYSTEM_NCCL=ON \
      -DUSE_DISTRIBUTED=ON \
      -DCMAKE_BUILD_TYPE=Release \
      -DCMAKE_INSTALL_PREFIX=../pytorch-install \
      -DCMAKE_C_COMPILER=$HOME/local/gcc-14.2.0/bin/gcc \
      -DCMAKE_CXX_COMPILER=$HOME/local/gcc-14.2.0/bin/g++ \
      -DCMAKE_CXX_FLAGS="-Wno-error=maybe-uninitialized -Wno-error=dangling-pointer -Wno-error=deprecated-declarations -mno-avx512f" \
      -DUSE_FBGEMM=OFF \
      -GNinja \
      ../pytorch



```

```bash
cmake -DCMAKE_C_COMPILER=$HOME/local/gcc-14.2.0/bin/gcc -DCMAKE_PREFIX_PATH=$HOME/hpcproject/pytorch-install ..
```

Rebuild torch with gcc libraries instead of 

Create new environment with python without sourcing the default things
And build the torch there


Or consider linking the gcc libraries when building instead or when running cmake


$HOME/torchenv/bin/python



```bash

mkdir build
cd build

cmake -DCMAKE_BUILD_TYPE=Release \
      -DCMAKE_INSTALL_PREFIX=$HOME/hpcproject/pytorch-install \
      -DCMAKE_C_COMPILER=$HOME/local/gcc-14.2.0/bin/gcc \
      -DCMAKE_CXX_COMPILER=$HOME/local/gcc-14.2.0/bin/g++ \
      -DCMAKE_PREFIX_PATH=$HOME/hpcproject/pytorch-install \
      -DCMAKE_EXE_LINKER_FLAGS="-Wl,-rpath,$HOME/local/gcc-14.2.0/lib64" ..


cmake --build . --config Release


```


```bash
cmake -DCMAKE_BUILD_TYPE=Release \
      -DCMAKE_INSTALL_PREFIX=$HOME/hpcproject/pytorch-install \
      -DCMAKE_C_COMPILER=$HOME/local/gcc-14.2.0/bin/gcc \
      -DCMAKE_CXX_COMPILER=$HOME/local/gcc-14.2.0/bin/g++ \
      -DCMAKE_PREFIX_PATH=$HOME/hpcproject/pytorch-install \
      -DCMAKE_INCLUDE_DIRS=$HOME/hpcproject/pytorch-install/include \
      -DCMAKE_EXE_LINKER_FLAGS="-Wl,-rpath,$HOME/local/gcc-14.2.0/lib64" ..
```


```bash

cmake -DCMAKE_PREFIX_PATH=$HOME/hpcproject/pytorch-install  -DCMAKE_BUILD_TYPE=Release       -DCMAKE_C_COMPILER=$HOME/local/gcc-14.2.0/bin/gcc       -DCMAKE_CXX_COMPILER=$HOME/local/gcc-14.2.0/bin/g++      -DCMAKE_EXE_LINKER_FLAGS="-Wl,-rpath,$HOME/local/gcc-14.2.0/lib64" ..

```


```bash
cmake -DCMAKE_PREFIX_PATH=$HOME/hpcproject/pytorch-install       -DCMAKE_C_COMPILER=icx       -DCMAKE_CXX_COMPILER=icpx      -DCMAKE_EXE_LINKER_FLAGS="-Wl,-rpath,$HOME/local/gcc-14.2.0/lib64" ..


```

