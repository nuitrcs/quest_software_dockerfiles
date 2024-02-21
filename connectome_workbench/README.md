## Instructions

You must have the followed files in the working directory `freesurfer-linux-ubuntu18_amd64-7.3.2.tar.gz` which can be obtained via this command.
```
wget https://surfer.nmr.mgh.harvard.edu/pub/dist/freesurfer/7.3.2/freesurfer-linux-ubuntu18_amd64-7.3.2.tar.gz
```
and a freesurfer `license.txt` file which can be obtained by going to the freesurfer website and requesting one.

## Build
```
docker build -f Dockerfile -t connectome:1.5.0 .
```
