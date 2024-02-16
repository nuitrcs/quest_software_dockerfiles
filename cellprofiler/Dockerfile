FROM mambaorg/micromamba

ARG DEBIAN_FRONTEND=noninteractive

USER root

RUN apt-get update && apt-get install libxxf86vm-dev --no-install-recommends -y

USER mambauser
COPY --chown=$MAMBA_USER:$MAMBA_USER cellprofiler.yaml /tmp/env.yaml
RUN micromamba install -y -n base -f /tmp/env.yaml && \
    micromamba clean --all --yes

ENTRYPOINT ["/usr/local/bin/_entrypoint.sh"]