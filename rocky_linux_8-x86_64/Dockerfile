FROM rockylinux:8

ENV DPDK_VERSION=v22.11.1

RUN dnf install -y 'dnf-command(config-manager)'
RUN dnf config-manager --set-enabled powertools

RUN yum update -y

RUN yum install -y \
	https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm

RUN yum install -y \
	autoconf \
	automake \
	clang \
	CUnit-devel \
	curl \
	diffutils \
	gcc \
	gcc-c++ \
	gcc-toolset-12-libatomic-devel \
	git-core \
	kmod \
	libatomic \
	libconfig-devel \
	libibverbs-devel \
	libpcap-devel \
	libtool \
	make \
	meson \
	numactl-devel \
	net-tools \
	ninja-build \
	openssl-devel \
	pkgconfig \
	python3-pip \
	procps \
	sudo

RUN pip3 install \
	pyelftools

RUN cd $HOME && \
    git clone http://dpdk.org/git/dpdk-stable --branch ${DPDK_VERSION} --depth 1 ./dpdk && \
    cd dpdk && \
    meson setup build && \
    cd build && \
    meson configure -Dmachine=default && \
    meson configure -Ddisable_apps=* && \
    meson configure -Dtests=false && \
    ninja && \
    ninja install && \
    cd $HOME && \
    rm -r ./dpdk
