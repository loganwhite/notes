https://github.com/NetSys/bess/issues/803 Building on CentOS 7 is tricky because some of its packages (such as gcc) are too old. Luckily, it provides developer toolsets that can be used in such situations. We've managed to compile BESS for CentOS and RHEL using the following commands (I'm pulling these out of a set of build scripts so let me know if you run into any issues):

Install dependencies with Yum:
```
yum -y install epel-release centos-release-scl
yum -y install libpcap-devel zlib-devel c-ares-devel openssl-devel \
    libunwind-devel numactl-devel glog-devel cmake kernel-headers kernel-devel \
    rpm-build devtoolset-4-gcc devtoolset-4-gcc-c++ git19
```
Install Python dependencies
```
pip install grpcio==1.11.0 scapy==2.4.0
```
Enable software collections:
```
scl enable devtoolset-4 git19 bash
```
Build dependencies from source:
```
# Clone gRPC
git clone https://github.com/google/grpc

# Compile and Install gRPC
pushd grpc
git checkout tags/v1.3.2  # Use version 1.3.2
git submodule update --init  # Download dependencies
make -j$(nproc) HAS_SYSTEM_PROTOBUF=false
sudo make install
popd

# Compile and Install protobuf
pushd grpc/third_party/protobuf
sudo make install
popd

# Compile and Install libbenchmark
pushd grpc/third_party/benchmark
cmake .
sudo make install
popd

# Compile and Install googletest (gtest)
pushd grpc/third_party/googletest
mkdir build
cd build
cmake ..
make -j$(nproc)
sudo make install
popd

# Compile and Install gflags
pushd grpc/third_party/gflags
mkdir build
cd build
cmake -DGFLAGS_NAMESPACE=google ..
make -j$(nproc)
sudo make install
popd
```
Build BESS:
```
# Clone BESS
git clone https://github.com/NetSys/bess.git
cd bess
git checkout $BESS_COMMIT_ID   # We use 811ef1ce6317cb9557b0270a1f2e908861f8921b

# Patch BESS - you'll need to modify this to match the path where you installed grpc. Mine was installed in /opt/
sed -i 's@GTEST_DIR := /usr/src/gtest@GTEST_DIR := /opt/grpc/third_party/googletest/googletest@g' core/Makefile

# Create symlink to kernel headers - if I remember correctly, this is needed to build the bess kmod
mkdir -p /lib/modules/$(uname -r)
ln -s /usr/src/kernels/$(rpm -qa | grep kernel-headers | sed -e 's/kernel-headers-//')/ /lib/modules/$(uname -r)/build

# We had some issues linking statically. It is possible with more patching of BESS but let's try to keep it simple for now ;)
BESS_LINK_DYNAMIC=1 python build.py
```
