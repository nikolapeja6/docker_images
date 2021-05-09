
# base image
FROM ubuntu:18.04

# install additional tools - standard
RUN apt-get update -y && apt install -y --no-install-recommends \
    git \
    graphviz \
    graphviz-dev \
    pkg-config \
    coinor-cbc \
    cmake \
    make \
    python \
    gcc \
    g++ \
    ca-certificates \
    && apt-get update -y \
    && update-ca-certificates \
    && rm -rf /var/lib/apt/lists/*


# install additional tools - manual
RUN git clone https://github.com/llvm/llvm-project --branch release/6.x --depth 1

# compile
RUN mkdir -p llvm-project/llvm/_build llvm-project/llvm/tools/ && cp -r llvm-project/polly llvm-project/llvm/tools/ && cp -r llvm-project/clang llvm-project/llvm/tools/
RUN cd llvm-project/llvm/_build && cmake .. -DCMAKE_BUILD_TYPE=Debug -DLLVM_INSTALL_UTILS=ON -DLLVM_TARGETS_TO_BUILD="X86" -DCMAKE_INSTALL_PREFIX=$HOME/llvm-6.0 \
    && make \
    && make install \
    && cd ../../../ && rm -rf llvm-project

####################
# dhls
#################
RUN git clone https://github.com/lana555/dynamatic.git && cd dynamatic && git checkout b1f302209bb6b2185f0525cc41fb2da1d1773a5b

# elastic pass
RUN cd dynamatic/elastic-circuits &&  mkdir _build && cd _build && cmake .. -DLLVM_ROOT=$HOME/llvm-6.0 && make
# change the configs 
RUN sed -ie 's/CLANG = .*/CLANG = \/root\/llvm-6.0\/bin\/clang/g' dynamatic/elastic-circuits/examples/Makefile && sed -ie 's/OPT = .*/OPT = \/root\/llvm-6.0\/bin\/opt/g' dynamatic/elastic-circuits/examples/Makefile


# Buffers
RUN cd dynamatic/Buffers && mkdir -p bin && make


####################
# run
######################

RUN cd dynamatic/elastic-circuits/examples/ && make name=loop_array  graph
#RUN echo $(find dynamatic/ -name 'loop_array*.dot') && false
#RUN dynamatic/Buffers/bin/buffers  buffers -filename=/dynamatic/elastic-circuits/_build/loop_array/loop_array_graph  -period=5