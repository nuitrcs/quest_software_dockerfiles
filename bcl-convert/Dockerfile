FROM centos:centos7

WORKDIR /etc/yum.repos.d/
RUN sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*
RUN sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*
RUN yum update -y

RUN yum install gdb -y

RUN mkdir -p /opt
WORKDIR /opt
COPY ./bcl-convert-4.3.6-2.el7.x86_64.rpm /opt/bcl-convert-4.3.6-2.el7.x86_64.rpm
RUN rpm --install bcl-convert-4.3.6-2.el7.x86_64.rpm

