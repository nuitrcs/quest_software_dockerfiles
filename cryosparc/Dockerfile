# syntax=docker/dockerfile:1
FROM nuitrcs/quest-ood:ubuntu2204

ENV DEBIAN_FRONTEND noninteractive

# Install MongoDB
RUN apt-get update -y && apt-get install -y gnupg curl ed
RUN curl -fsSL https://www.mongodb.org/static/pgp/server-6.0.asc | \
   gpg -o /usr/share/keyrings/mongodb-server-6.0.gpg \
   --dearmor

RUN echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-6.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/6.0 multiverse" | tee /etc/apt/sources.list.d/mongodb-org-6.0.list
RUN apt-get update -y
RUN apt-get install -y mongodb-org

# Install regular dependencies of CRYOSPARC
RUN apt-get install -y --no-install-recommends \
    iputils-ping \
    python3 \
    python3-dev \
    python3-setuptools \
    python3-pip

# A place to install CRYOSPARC
ENV CRYOSPARC_ROOT_DIR /app
RUN mkdir -p ${CRYOSPARC_ROOT_DIR}
WORKDIR ${CRYOSPARC_ROOT_DIR}

ARG CRYOSPARC_VERSION
ENV CRYOSPARC_VERSION=${CRYOSPARC_VERSION}

# install CRYOSPARC master
ENV CRYOSPARC_MASTER_DIR ${CRYOSPARC_ROOT_DIR}/cryosparc_master
RUN --mount=type=secret,id=CRYOSPARC_LICENSE_ID \
  curl -L https://get.cryosparc.com/download/master-v${CRYOSPARC_VERSION}/$(cat /run/secrets/CRYOSPARC_LICENSE_ID) | tar -xz \
	&& cd ${CRYOSPARC_MASTER_DIR} \
  && sed -i '/^echo "# Other" >> config.sh$/a echo \"export CRYOSPARC_FORCE_USER=true\" >> config.sh' ./install.sh \
  && bash ./install.sh --license "$(cat /run/secrets/CRYOSPARC_LICENSE_ID)" --yes --allowroot \
  && sed -i 's/^export CRYOSPARC_LICENSE_ID=.*$/export CRYOSPARC_LICENSE_ID=TBD/g' ${CRYOSPARC_MASTER_DIR}/config.sh 

ARG CRYOSPARC_PATCH
ENV CRYOSPARC_PATCH=${CRYOSPARC_PATCH}

# update patches
RUN if [ ! -z "${CRYOSPARC_PATCH}" ]; then curl -L https://get.cryosparc.com/patch_get/v${CRYOSPARC_VERSION}+${CRYOSPARC_PATCH}/master -o ${CRYOSPARC_ROOT_DIR}/cryosparc_master_patch.tar.gz \
  && tar -vxzf ${CRYOSPARC_ROOT_DIR}/cryosparc_master_patch.tar.gz --overwrite --strip-components=1 --directory=${CRYOSPARC_MASTER_DIR} \
  && rm -f ${CRYOSPARC_ROOT_DIR}/cryosparc_master_patch.tar.gz; fi

# patches
RUN sed -i 's:    disk_has_space=.*:    disk_has_space="true":g'  ${CRYOSPARC_MASTER_DIR}/bin/cryosparcm

# install worker
ENV CRYOSPARC_WORKER_DIR ${CRYOSPARC_ROOT_DIR}/cryosparc_worker
RUN --mount=type=secret,id=CRYOSPARC_LICENSE_ID \
  curl -L https://get.cryosparc.com/download/worker-v${CRYOSPARC_VERSION}/$(cat /run/secrets/CRYOSPARC_LICENSE_ID) | tar -xz \
  && cd ${CRYOSPARC_WORKER_DIR} \
  && bash ./install.sh --license "$(cat /run/secrets/CRYOSPARC_LICENSE_ID)" --yes \
  && sed -i '/^echo "# Other" >> config.sh$/a echo \"export CRYOSPARC_FORCE_USER=true\" >> config.sh' ./install.sh \
  && sed -i 's/^export CRYOSPARC_LICENSE_ID=.*$/export CRYOSPARC_LICENSE_ID=TBD/g' ${CRYOSPARC_WORKER_DIR}/config.sh 

# update patches
RUN if [ ! -z "${CRYOSPARC_PATCH}" ]; then curl -L https://get.cryosparc.com/patch_get/v${CRYOSPARC_VERSION}+${CRYOSPARC_PATCH}/worker -o ${CRYOSPARC_ROOT_DIR}/cryosparc_worker_patch.tar.gz \
  && tar -vxzf ${CRYOSPARC_ROOT_DIR}/cryosparc_worker_patch.tar.gz --overwrite --strip-components=1 --directory=${CRYOSPARC_WORKER_DIR} \
  && rm -f ${CRYOSPARC_ROOT_DIR}/cryosparc_worker_patch.tar.gz; fi

# install cryosparc live
ARG CRYOSPARC_LIVE
RUN --mount=type=secret,id=CRYOSPARC_LICENSE_ID \
  if [ ! -z $CRYOSPARC_LIVE ]; then cd ${CRYOSPARC_MASTER_DIR} \
    curl -L "https://get.cryosparc.com/download/master-${CRYOSPARC_LIVE}/$(cat /run/secrets/CRYOSPARC_LICENSE_ID)" | tar -xz --overwrite --strip-components=1 --directory . \
    && ${CRYOSPARC_MASTER_DIR}/bin/cryosparcm deps \
    && sed -i 's/^export CRYOSPARC_LICENSE_ID=.*$/export CRYOSPARC_LICENSE_ID=TBD/g' ${CRYOSPARC_MASTER_DIR}/config.sh; \
  fi
RUN --mount=type=secret,id=CRYOSPARC_LICENSE_ID \
  if [ ! -z $CRYOSPARC_LIVE ]; then cd ${CRYOSPARC_WORKER_DIR} \
    curl -L "https://get.cryosparc.com/download/worker-${CRYOSPARC_LIVE}/$(cat /run/secrets/CRYOSPARC_LICENSE_ID)" | tar -xz --overwrite --strip-components=1 --directory . \
    && ${CRYOSPARC_WORKER_DIR}/bin/cryosparcw deps \
    && sed -i 's/^export CRYOSPARC_LICENSE_ID=.*$/export CRYOSPARC_LICENSE_ID=TBD/g' ${CRYOSPARC_WORKER_DIR}/config.sh; \
  fi
