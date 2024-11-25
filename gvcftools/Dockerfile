FROM centos:6

RUN curl https://www.getpagespeed.com/files/centos6-eol.repo --output /etc/yum.repos.d/CentOS-Base.repo
RUN yum -y install git gcc-c++ zlib-devel

WORKDIR /opt
RUN git clone https://github.com/sequencing/gvcftools.git

WORKDIR /opt/gvcftools
RUN make
