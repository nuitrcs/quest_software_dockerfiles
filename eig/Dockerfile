FROM ubuntu:20.04

RUN apt update
RUN DEBIAN_FRONTEND=noninteractive apt install -y git build-essential pkg-config libopenblas-dev liblapacke-dev libgsl-dev

WORKDIR /opt
RUN git clone https://github.com/DReichLab/EIG.git

WORKDIR /opt/EIG/src
RUN LDLIBS="-llapacke" make
RUN make install

