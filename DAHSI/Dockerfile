FROM ubuntu:20.04

RUN apt update
RUN apt install -y software-properties-common
RUN add-apt-repository -y ppa:deadsnakes/ppa
RUN DEBIAN_FRONTEND=noninteractive apt install -y git wget gfortran python3.9 python-is-python3 \
python3-pip build-essential manpages-dev pkg-config libblas-dev liblapack-dev

WORKDIR /opt
RUN git clone https://github.com/hribera/DAHSI.git
WORKDIR /opt/DAHSI
RUN python -m pip install -r requirements.txt

RUN mkdir MainIpopt
WORKDIR /opt/DAHSI/MainIpopt
RUN git clone https://github.com/coin-or-tools/ThirdParty-ASL.git
WORKDIR /opt/DAHSI/MainIpopt/ThirdParty-ASL
RUN ./get.ASL
RUN ./configure
RUN make clean
RUN make
RUN make install

WORKDIR /opt/DAHSI/MainIpopt
RUN git clone https://github.com/coin-or-tools/ThirdParty-HSL.git
WORKDIR /opt/DAHSI/MainIpopt/ThirdParty-HSL
COPY ./coinhsl-2023.11.17.tar.gz .
RUN tar -xzf coinhsl-2023.11.17.tar.gz
RUN mv coinhsl-2023.11.17 coinhsl
RUN ./configure
RUN make clean
RUN make
RUN make install

WORKDIR /opt/DAHSI/MainIpopt
RUN git clone https://github.com/coin-or-tools/ThirdParty-Mumps.git
WORKDIR /opt/DAHSI/MainIpopt/ThirdParty-Mumps
RUN ./get.Mumps
RUN ./configure
RUN make clean
RUN make
RUN make install

WORKDIR /opt/DAHSI/MainIpopt
RUN git clone --depth 1 --branch releases/3.14.10 https://github.com/coin-or/Ipopt.git
WORKDIR /opt/DAHSI/MainIpopt/Ipopt
RUN mkdir build
WORKDIR /opt/DAHSI/MainIpopt/Ipopt/build
RUN ../configure
RUN make clean
RUN make
RUN make test
RUN make install
RUN export PKG_CONFIG_PATH=/opt/DAHSI/MainIpopt/Ipopt/build/src:$PKG_CONFIG_PATH

WORKDIR /opt/DAHSI/MainIpopt
RUN git clone --depth 1 --branch v1.2.0 https://github.com/mechmotum/cyipopt.git
WORKDIR /opt/DAHSI/MainIpopt/cyipopt
RUN python setup.py install

ENV LD_LIBRARY_PATH="/usr/local/lib/"
