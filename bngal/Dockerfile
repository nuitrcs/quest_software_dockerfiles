FROM mambaorg/micromamba
ARG DEBIAN_FRONTEND=noninteractive

# build bngal environment from yml
COPY --chown=$MAMBA_USER:$MAMBA_USER bngal.yml /tmp/env.yaml
RUN micromamba install -y -n base -f /tmp/env.yaml && \
    micromamba clean --all --yes

ARG start_here=1

# install git as root
USER root
RUN apt-get update && apt-get install -y --no-install-recommends git
USER $MAMBA_USER
ARG MAMBA_DOCKERFILE_ACTIVATE=1 

# clone github and create pipeline executables
RUN git clone https://github.com/mselensky/bngal-cli.git $HOME/bngal-cli
RUN cd $HOME/bngal-cli && \
  cp R/bngal-build-networks.R ${CONDA_PREFIX}/bin/bngal-build-nets
RUN cd $HOME/bngal-cli && \
  cp R/bngal-summarize-networks.R ${CONDA_PREFIX}/bin/bngal-summarize-nets
RUN chmod +x ${CONDA_PREFIX}/bin/bngal-build-nets \
  && chmod +x ${CONDA_PREFIX}/bin/bngal-summarize-nets

# install remaining R packages
RUN R -e 'if (!require("bngal")) remotes::install_github("mselensky/bngal", upgrade="never")' &> R-pkgs-install.log

WORKDIR "/home/mambauser"

ENTRYPOINT ["/usr/local/bin/_entrypoint.sh"]


