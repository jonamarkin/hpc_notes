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


```

```bash
cmake -DCMAKE_C_COMPILER=$HOME/local/gcc-14.2.0/bin/gcc -DCMAKE_PREFIX_PATH=$HOME/hpcproject/pytorch-install ..
```

Rebuild torch with gcc libraries instead of 

Create new environment with python without sourcing the default things
And build the torch there
