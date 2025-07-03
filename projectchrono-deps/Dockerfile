FROM nvidia/cuda:11.8.0-devel-ubuntu22.04

# apt-get some stuff 
RUN apt-get update \ 
    && DEBIAN_FRONTEND=noninteractive apt-get -y install wget git cmake python-is-python3 ninja-build build-essential libboost-dev swig libeigen3-dev libglfw3-dev libglm-dev libglew-dev freeglut3-dev libirrlicht-dev libxxf86vm-dev python3 python3-dev libhdf5-dev libnvidia-gl-510 make file libnuma-dev gdb

######
## Follow these directions to install intel-mkl nicely 
## https://www.intel.com/content/www/us/en/developer/tools/oneapi/onemkl-download.html?operatingsystem=linux&linux-install=apt

# To add APT repository access, install the prerequisites
RUN apt update
RUN apt install -y gpg-agent wget

# Set up the repository. To do this, download the key to the system keyring
RUN wget -O- https://apt.repos.intel.com/intel-gpg-keys/GPG-PUB-KEY-INTEL-SW-PRODUCTS.PUB \ 
    | gpg --dearmor | tee /usr/share/keyrings/oneapi-archive-keyring.gpg > /dev/null

# Add the signed entry to APT sources and 
# configure the APT client to use the Intel repository
RUN echo "deb [signed-by=/usr/share/keyrings/oneapi-archive-keyring.gpg] https://apt.repos.intel.com/oneapi all main" \ 
    | tee /etc/apt/sources.list.d/oneAPI.list

# Update the packages list and repository index
RUN apt update

# For running applications that require oneMKL
RUN apt install -y intel-oneapi-mkl

# For developing and compiling oneMKL applications
RUN apt install -y intel-oneapi-mkl-devel

# oneMKL runtime package for C/Fortran functionality and Cluster components
RUN apt install -y intel-oneapi-mkl-classic

# oneMKL development package for C/Fortran functionality and Cluster components
RUN apt install -y intel-oneapi-mkl-classic-devel
######

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
RUN git clone https://bitbucket.org/blaze-lib/blaze.git --branch v3.8
RUN cp -r /blaze /usr/local/include
RUN git clone --depth 1 --branch 7.0.3 https://github.com/projectchrono/chrono.git
WORKDIR chrono
RUN git submodule init
RUN git submodule update
RUN mkdir -p build
# Make flatbuffers
WORKDIR /chrono/src/chrono_thirdparty/flatbuffers
RUN cmake -G "Ninja" -DCMAKE_BUILD_TYPE=Release .
RUN ninja -j 4
