FROM mambaorg/micromamba
ARG DEBIAN_FRONTEND=noninteractive

# build tedana environment from yml
COPY --chown=$MAMBA_USER:$MAMBA_USER tedana.yml /tmp/env.yaml
RUN micromamba install -y -n base -f /tmp/env.yaml && \
    micromamba clean --all --yes
USER $MAMBA_USER
ARG MAMBA_DOCKERFILE_ACTIVATE=1 

WORKDIR "/home/mambauser"

ENTRYPOINT ["/usr/local/bin/_entrypoint.sh"]


