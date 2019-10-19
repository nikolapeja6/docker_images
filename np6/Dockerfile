
# base image
FROM ubuntu:18.04

# Update Ubuntu Software repository
RUN apt-get update -y

# install additional tools - standard
RUN apt install git -y
RUN apt install graphviz -y
RUN apt install graphviz-dev -y
RUN apt install pkg-config -y
RUN apt install coinor-cbc -y
RUN apt install cmake -y
RUN apt install make -y
RUN apt install python -y

# install additional tools - manual
# download
RUN git clone http://llvm.org/git/llvm.git --branch release_60 --depth 1
RUN cd llvm/tools && git clone http://llvm.org/git/clang.git --branch release_60 --depth 1 
RUN cd llvm/tools && git clone http://llvm.org/git/polly.git --branch release_60 --depth 1
# compile
RUN cd llvm && mkdir _build
RUN cd llvm/_build && cmake .. -DCMAKE_BUILD_TYPE=RelWithDebInfo -DLLVM_INSTALL_UTILS=ON -DLLVM_TARGETS_TO_BUILD="X86" -DCMAKE_INSTALL_PREFIX=$HOME/llvm-6.0
RUN cd llvm/_build && make
RUN cd llvm/_build && make install
