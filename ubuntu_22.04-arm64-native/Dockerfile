FROM ubuntu:22.04

ENV DPDK_VERSION=v22.11.1 \
    IPSEC_MB_VERSION=SECLIB-IPSEC-2022.12.13 \
    XDP_VERSION=v1.2.3 \
    AARCH64_CRYPTOLIB_TAG=2445f0e

RUN apt-get update

RUN apt-get install -yy --no-install-recommends \
        software-properties-common \
	dirmngr \
	gnupg-agent

RUN apt-get update --fix-missing

RUN apt-get install -yy \
  autoconf \
  automake \
  ccache \
  clang \
  gcc \
  gcc-10 \
  gcc-11 \
  gcc-12 \
  git \
  iproute2 \
  kmod \
  libbpf-dev \
  libcli-dev \
  libconfig-dev \
  libcunit1-dev \
  libpcap-dev \
  libssl-dev \
  libstdc++-10-dev \
  libstdc++-11-dev \
  libstdc++-12-dev \
  libtool \
  llvm-dev \
  meson \
  nasm \
  net-tools \
  ninja-build \
  python3-pip \
  sudo

RUN pip3 install \
  pyelftools

RUN cd $HOME && \
    git clone http://dpdk.org/git/dpdk-stable --branch ${DPDK_VERSION} --depth 1 ./dpdk && \
    cd dpdk && \
    meson setup build && \
    cd build && \
    meson configure -Ddisable_apps=* && \
    meson configure -Dtests=false && \
    ninja && \
    ninja install && \
    cd $HOME && \
    rm -r ./dpdk

RUN cd $HOME && \
    git clone https://github.com/ARM-software/AArch64cryptolib --depth 1 ./aarch64cryptolib && \
    cd aarch64cryptolib && \
    git checkout ${AARCH64_CRYPTOLIB_TAG} && \
    make OPT=big && \
    cd -

RUN cd $HOME && \
    git clone https://git.gitlab.arm.com/arm-reference-solutions/ipsec-mb.git --branch ${IPSEC_MB_VERSION} --depth 1 ./ipsec-mb && \
    cd ipsec-mb && \
    make -j $(nproc) && \
    make install && \
    cd $HOME && \
    rm -r ./ipsec-mb

RUN cd $HOME && \
    git clone --recurse-submodules https://github.com/xdp-project/xdp-tools.git --branch ${XDP_VERSION} --depth 1 ./xdp && \
    cd xdp && \
    ./configure && \
    C_INCLUDE_PATH=/usr/include/aarch64-linux-gnu/ make && \
    make install && \
    ldconfig /usr/local/lib/ && \
    cd $HOME && \
    rm -r ./xdp
