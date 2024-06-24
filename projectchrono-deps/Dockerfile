FROM nvidia/cuda:11.7.1-devel-ubuntu22.04

# Making a change to trigger the build and push
RUN apt-get update \ 
    && DEBIAN_FRONTEND=noninteractive apt-get -y install wget python git cmake ninja-build build-essential libboost-dev swig libeigen3-dev libglfw3-dev libglm-dev libglew-dev freeglut3-dev libirrlicht-dev libxxf86vm-dev python3 python3-dev libhdf5-dev libnvidia-gl-510 make file libnuma-dev gdb intel-mkl

# Install OpenMPI the way that it is installed on Quest
# Start with UCX

RUN wget https://github.com/openucx/ucx/releases/download/v1.10.0/ucx-1.10.0.tar.gz
RUN tar xzf ucx-1.10.0.tar.gz
RUN mkdir -p /ucx-1.10.0/build
WORKDIR /ucx-1.10.0/build
RUN ../contrib/configure-release --prefix=/usr/local/ucx-1.10.0
RUN make -j4 install

RUN echo "Installing Open MPI"
ENV OMPI_DIR=/opt/ompi
ENV OMPI_VERSION=4.1.4
ENV OMPI_URL="https://download.open-mpi.org/release/open-mpi/v4.1/openmpi-$OMPI_VERSION.tar.bz2"
RUN mkdir -p /tmp/ompi
RUN mkdir -p /opt

# Download
RUN cd /tmp/ompi && wget -O openmpi-$OMPI_VERSION.tar.bz2 $OMPI_URL && tar -xjf openmpi-$OMPI_VERSION.tar.bz2
# Compile and install
RUN cd /tmp/ompi/openmpi-$OMPI_VERSION && ./configure --prefix=$OMPI_DIR --enable-mca-no-build=btl-uct --with-ucx=/usr/local/ucx-1.10.0 && make -j4 install

# Go back to root
WORKDIR /

# Set env variables so we can compile our application
ENV PATH=$OMPI_DIR/bin:$PATH
ENV LD_LIBRARY_PATH=$OMPI_DIR/lib:$LD_LIBRARY_PATH
RUN ldconfig
RUN wget https://bitbucket.org/blaze-lib/blaze/downloads/blaze-3.8.tar.gz
RUN tar -xf blaze-3.8.tar.gz
RUN cp -r blaze-3.8/blaze /usr/local/include
RUN git clone --depth 1 --branch 7.0.3 https://github.com/projectchrono/chrono.git
WORKDIR chrono
RUN git submodule init
RUN git submodule update
RUN mkdir -p build
# Make flatbuffers
WORKDIR /chrono/src/chrono_thirdparty/flatbuffers
RUN cmake -G "Ninja" -DCMAKE_BUILD_TYPE=Release .
RUN ninja -j 4
