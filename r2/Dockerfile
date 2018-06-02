FROM ubuntu:16.04

# Dependencies
RUN apt-get update && \
    apt-get -y install \
	libcurl4-gnutls-dev \
	build-essential \
	git \
	perl \
	python3 \
	bash \
	bison \
	flex \
	autoconf \
	automake \
	libtool \
	pkg-config \
	m4 \
	coreutils \
	zlib1g-dev \
	libtinfo-dev \
	wget \
	bc \
	upx \
	doxygen \
	graphviz \
	python3-dev \
	ca-certificates \
    --no-install-recommends \
    && rm -rf /var/lib/apt/lists/*

# Get latest radare2 release and build it
WORKDIR /opt
RUN git clone https://github.com/radare/radare2 \
	&& cd radare2 && ./sys/install.sh

# Get cmake
WORKDIR /opt
RUN git clone -b v3.10.2 https://cmake.org/cmake.git cmake
WORKDIR /opt/cmake
RUN ./bootstrap --system-curl \
	&& make -j`grep -c ^processor /proc/cpuinfo` \
	&& make install

# Get latest retdec
WORKDIR /opt
RUN git clone https://github.com/avast-tl/retdec \
	&& mkdir /opt/retdec/build \
	&& mkdir /opt/retdec-install

WORKDIR /opt/retdec/build
RUN cmake .. -DCMAKE_INSTALL_PREFIX=/opt/retdec-install \
	&& make -j`grep -c ^processor /proc/cpuinfo` \
	&& make install

RUN wget -O - https://deb.nodesource.com/setup_10.x | bash - \
	&& apt-get install -yq nodejs

# Add r2 user
RUN useradd r2

WORKDIR /home/r2
RUN mkdir -p /home/r2/.config/radare2 \
	&& touch /home/r2/.radare2rc

RUN chown -R r2:r2 /home/r2/
USER r2

RUN r2pm init \
	&& r2pm -i r2com \
	&& r2pm -i r2retdec \
	&& echo "/opt/retdec-install/bin/retdec-decompiler.sh" > .r2retdec

ENTRYPOINT ["/bin/bash"]

