FROM ubuntu:18.04

# Update and install dependencies
RUN apt-get -y update && apt-get -y install \
                                    qtbase5-dev \
                                    libqt5opengl5-dev \
                                    zlib1g-dev \
                                    libosmesa6-dev \
                                    libssl-dev \
                                    libqwt-dev \
                                    libfreetype6-dev \
                                    libftgl-dev \
                                    cmake \
                                    git \
                                    g++ \
                                    libglib2.0-dev \
                                    ffmpeg

RUN git clone https://github.com/Washington-University/workbench.git /opt/src/workbench
WORKDIR /opt/src/workbench
RUN git checkout v1.5.0
RUN mkdir build
WORKDIR build
RUN cmake -DCMAKE_BUILD_TYPE=Release -DWORKBENCH_MESA_DIR=/usr -DWORKBENCH_USE_QT5=TRUE -DCMAKE_INSTALL_PREFIX=/opt/workbench ../src
RUN make -j4
RUN make install

WORKDIR /

# install fs
COPY freesurfer-linux-ubuntu18_amd64-7.3.2.tar.gz /fs.tar.gz
RUN tar --no-same-owner -xzvf fs.tar.gz && \
    mv freesurfer /usr/local && \
    rm fs.tar.gz

# setup fs env
ENV OS Linux
ENV PATH /usr/local/freesurfer/bin:/usr/local/freesurfer/fsfast/bin:/usr/local/freesurfer/tktools:/usr/local/freesurfer/mni/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
ENV FREESURFER_HOME /usr/local/freesurfer
ENV FREESURFER /usr/local/freesurfer
ENV SUBJECTS_DIR /usr/local/freesurfer/subjects
ENV LOCAL_DIR /usr/local/freesurfer/local
ENV FSFAST_HOME /usr/local/freesurfer/fsfast
ENV FMRI_ANALYSIS_DIR /usr/local/freesurfer/fsfast
ENV FUNCTIONALS_DIR /usr/local/freesurfer/sessions

# set default fs options
ENV FS_OVERRIDE 0
ENV FIX_VERTEX_AREA ""
ENV FSF_OUTPUT_FORMAT nii.gz

# mni env requirements
ENV MINC_BIN_DIR /usr/local/freesurfer/mni/bin
ENV MINC_LIB_DIR /usr/local/freesurfer/mni/lib
ENV MNI_DIR /usr/local/freesurfer/mni
ENV MNI_DATAPATH /usr/local/freesurfer/mni/data
ENV MNI_PERL5LIB /usr/local/freesurfer/mni/share/perl5
ENV PERL5LIB /usr/local/freesurfer/mni/share/perl5

COPY license.txt /usr/local/freesurfer
