FROM centos:centos7

# Comment to trigger action
# Install vim, emacs, nano, tcl, and lua
RUN yum install -y epel-release
RUN yum -y install vim \
                   nano \
                   emacs \
                   tcl \
                   lua \
                   xorg-x11-xauth \
                   xorg-x11-xkb-utils\
                   gcc \
                   gcc-c++ \
                   gcc-gfortran \
                   make \
                   autoconf \
                   automake \
                   cmake

RUN yum install -y zlib-devel boost-devel mesa-libGLU-devel libXt-devel python-devel python3-devel tbb-devel eigen3-devel python-contextlib2 python3-contextlib2 python3-imageio python3-numpy python3-scipy python3-pandas python3-numexpr

RUN yum install -y git tar gunzip wget
RUN  wget -qO- "https://cmake.org/files/v3.27/cmake-3.27.0-linux-x86_64.tar.gz" | tar --strip-components=1 -xz -C /usr/local
RUN ln -s /usr/local/bin/cmake  /usr/local/bin/cmake3

RUN git clone -b LastDrawEMVersion --single-branch https://github.com/DevelopmentalImagingMCRI/MCRIBS.git
WORKDIR /MCRIBS
COPY ./build.sh /MCRIBS/build.sh
RUN bash ./build.sh
RUN yum install -y libjpeg-devel
RUN python3 -m pip install nibabel scipy pandas pandas imageio

# Clean up
RUN yum clean all
RUN rm -rf /var/cache/yum/*
