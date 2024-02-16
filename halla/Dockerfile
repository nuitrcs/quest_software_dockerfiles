FROM mambaorg/micromamba

ARG DEBIAN_FRONTEND=noninteractive

COPY --chown=$MAMBA_USER:$MAMBA_USER halla.yaml /tmp/env.yaml

RUN micromamba install -y -n base -f /tmp/env.yaml && \
    micromamba clean --all --yes

USER root
RUN apt-get update && apt-get install -y --no-install-recommends git

USER $MAMBA_USER
ARG MAMBA_DOCKERFILE_ACTIVATE=1
RUN git clone https://github.com/biobakery/halla.git $HOME/halla

RUN eval "$(micromamba shell hook --shell )"
# deal with sklearn deprecation error
ARG SKLEARN_ALLOW_DEPRECATED_SKLEARN_PACKAGE_INSTALL=True
RUN sed "s/sklearn/scikit-learn/" $HOME/halla/requirements.txt > $HOME/halla/requirements2.txt && mv $HOME/halla/requirements2.txt $HOME/halla/requirements.txt 

RUN cd $HOME/halla && pip install halla
RUN cd / && rm -r $HOME/halla

ENTRYPOINT ["/usr/local/bin/_entrypoint.sh"]
