# The Build Command
```
export CRYOSPARC_VERSION=4.5.3
export CRYOSPARC_PATCH=""
DOCKER_BUILDKIT=1 docker build \
        --build-arg CRYOSPARC_VERSION=${CRYOSPARC_VERSION} \
        --build-arg CRYOSPARC_PATCH=${CRYOSPARC_PATCH} \
        --progress=plain \
        --secret id=CRYOSPARC_LICENSE_ID \
        . -f Dockerfile \
        -t cryosparc-quest:${CRYOSPARC_VERSION}
```
