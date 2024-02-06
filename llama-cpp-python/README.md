# Build steps

## Clone the specific version of llama-cpp-python
```
git clone --depth 1 --branch <llama-cpp-version> https://github.com/abetlen/llama-cpp-python
```
## overwrite the cuda_simple Dockerfile with our Dockerfile
```
cp Dockerfile llama-cpp-python/docker/cuda_simple/Dockerfile
```
## build the container
```
cd llama-cpp-python 
docker build -f docker/cuda_simple/Dockerfile -t llama:<llama-cpp-version> .
```
