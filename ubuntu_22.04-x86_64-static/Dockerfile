FROM ubuntu:22.04

ENV DPDK_VERSION=v22.11.1

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
  gcc-9 \
  gcc-10 \
  gcc-11 \
  gcc-12 \
  git \
  kmod \
  libconfig-dev \
  libcunit1-dev \
  libssl-dev \
  libstdc++-9-dev \
  libstdc++-10-dev \
  libstdc++-11-dev \
  libstdc++-12-dev \
  libtool \
  llvm-dev \
  meson \
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
    meson configure -Dmachine=default && \
    meson configure -Ddefault_library=static && \
    meson configure -Ddisable_apps=* && \
    meson configure -Dtests=false && \
    ninja && \
    ninja install && \
    cd $HOME && \
    rm -r ./dpdk
