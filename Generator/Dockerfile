FROM centos:centos8

RUN yum install git make cmake gcc-c++ gcc binutils libX11-devel libXpm-devel libXft-devel libXext-devel python openssl-devel wget

RUN yum install epel-release
RUN yum config-manager --set-enabled powertools
RUN yum install redhat-lsb-core gcc-gfortran pcre-devel \
        mesa-libGL-devel mesa-libGLU-devel glew-devel ftgl-devel mysql-devel \
        fftw-devel cfitsio-devel graphviz-devel libuuid-devel \
        avahi-compat-libdns_sd-devel openldap-devel python3-numpy \
        libxml2-devel gsl-devel readline-devel qt5-qtwebengine-devel \
        R-devel R-Rcpp-devel R-RInside-devel gsl

RUN mkdir -p /opt
WORKDIR /opt
RUN wget https://root.cern/download/root_v6.28.04.Linux-centos8-x86_64-gcc8.5.tar.gz
RUN tar -xzvf root_v6.28.04.Linux-centos8-x86_64-gcc8.5.tar.gz

# Clean up
RUN yum clean all
RUN rm -rf /var/cache/yum/*
