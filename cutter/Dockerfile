FROM ubuntu:rolling

# Dependencies
RUN apt-get update && \
	apt-get -y install \
	curl \
	libqt5svg5-dev \
	make \
	qt5-default \
	qtbase5-dev \
	qtwebengine5-dev \
	unzip \
	wget \
	cmake \
	g++ \
	gcc \
	git-core \
	python3 \
	python3-dev \
	python3-pip \
	python3-setuptools \
	pkg-config \
	--no-install-recommends \
	&& rm -rf /var/lib/apt/lists/*

RUN pip3 install --upgrade jupyter

# Get latest cutter release
WORKDIR /opt
RUN git clone --depth=1 --recurse-submodules https://github.com/radareorg/cutter
WORKDIR /opt/cutter

# Install radare2
RUN cd radare2 && ./sys/install.sh

# Build cutter
# RUN sed -i "s/QMAKE_CONF=.*/QMAKE_CONF=\"CUTTER_ENABLE_JUPYTER=false CUTTER_ENABLE_QTWEBENGINE=false\"/gi" ./build.sh
RUN ./build.sh

# Add r2 user
RUN useradd r2

WORKDIR /home/r2
RUN mkdir -p /home/r2/.local/share/jupyter \
	&& mkdir -p /home/r2/.config/radare2 \
	&& touch /home/r2/.radare2rc \
	&& chown -R r2:r2 /home/r2/

USER r2

ENTRYPOINT ["/bin/bash", "-c", "/opt/cutter/build/Cutter"]

