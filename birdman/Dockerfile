FROM mambaorg/micromamba
ARG DEBIAN_FRONTEND=noninteractive

# build birdman environment from yml
COPY --chown=$MAMBA_USER:$MAMBA_USER birdman.yml /tmp/env.yaml
RUN micromamba install -y -n base -f /tmp/env.yaml && \
    micromamba clean --all --yes

ARG MAMBA_DOCKERFILE_ACTIVATE=1

RUN pip install birdman

WORKDIR "/home/mambauser"

ENTRYPOINT ["/usr/local/bin/_entrypoint.sh"]


