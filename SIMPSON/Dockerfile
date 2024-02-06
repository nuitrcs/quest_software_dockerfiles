FROM ubuntu:16.04 

RUN apt-get update \ 
    && DEBIAN_FRONTEND=noninteractive apt-get install -y build-essential tcl

COPY ./Simpson_Setup_Linux_4.2.1.tbz2 /Simpson_Setup_Linux_4.2.1.tbz2
RUN tar -xf Simpson_Setup_Linux_4.2.1.tbz2
RUN mv "Simpson Setup Linux 4.2.1" Simpson_Setup
WORKDIR Simpson_Setup
RUN ./install.sh --bindir=/opt/simpson/bin --libdir=/opt/simpson/lib
