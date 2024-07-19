# quest_software_dockerfiles

Julia's fork :-)
Development branch


A repository for all Dockerfiles or Singularity Definition files used for the purpose of installing a given software on Quest

Note: for any micromamba-docker-based containers converted to Singularity, you will have to add `/usr/local/bin/_entrypoint.sh` [before the command you wish to execute](https://micromamba-docker.readthedocs.io/en/latest/faq.html#how-can-i-use-a-mambaorg-micromamba-based-image-with-apptainer). For example:

`singularity exec micromamba-python.sif /usr/local/bin/_entrypoint.sh python --help`
