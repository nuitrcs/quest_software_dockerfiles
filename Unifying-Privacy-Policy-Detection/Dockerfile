FROM ubuntu:22.04

SHELL ["/bin/bash", "-c"]

# Update ubuntu and setup conda
# adapted from: https://hub.docker.com/r/conda/miniconda3/dockerfile
RUN sed -i'' 's/archive\.ubuntu\.com/us\.archive\.ubuntu\.com/' /etc/apt/sources.list
RUN apt-get clean -qq \
    && rm -r /var/lib/apt/lists/* -vf \
    && apt-get clean -qq \
    && apt-get update -qq \
    && apt-get upgrade -qq \
    && apt-get install wget git make -qq \
    && apt-get install psmisc firefox xvfb libgtk-3-dev libasound2 libdbus-glib-1-2 libpci3 -qq

RUN git clone https://github.com/openwpm/OpenWPM.git /opt/OpenWPM
ENV HOME /opt
WORKDIR /opt/OpenWPM
RUN ./scripts/install-mamba.sh
ENV PATH $HOME/mamba/bin:$PATH

# Install OpenWPM
RUN ./install.sh
ENV PATH $HOME/mamba/envs/openwpm/bin:$PATH
RUN eval "$($HOME/mamba/bin/conda shell.bash hook)" && conda activate openwpm && conda install --yes -c conda-forge cchardet && pip3 install ndjson tranco chardet urljoin lxml

# Move the firefox binary away from the /opt/OpenWPM root so that it is available if
# we mount a local source code directory as /opt/OpenWPM
RUN mv firefox-bin /opt/firefox-bin
ENV FIREFOX_BINARY /opt/firefox-bin/firefox-bin

RUN git clone https://github.com/ITSec-WWU-Munster/Unifying-Privacy-Policy-Detection.git
RUN cp Unifying-Privacy-Policy-Detection/privacy_policy_link_detection/run_crawler.sh .
RUN cp Unifying-Privacy-Policy-Detection/privacy_policy_link_detection/custom_command_find_privacy_policies.py .
RUN cp Unifying-Privacy-Policy-Detection/privacy_policy_link_detection/demo_privacy_policy_download.py .

# Setting run_crawler.sh as the default command
CMD [ "bash", "run_crawler.sh"]
