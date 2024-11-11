# Docker Cheet Sheet

### 1. Docker Build Commands
- `docker build -t <imagename> .`:build a docker image from a dockerfile in the current directory and tag it with a name
- `docker build --no-cache -t <imagename> .`: build a docker image without using the cache
- `docker build -f <dockerfilename> -t <imagename> .`: build a docker image using a specified dockerfile
- `docker build --build-arg <variablename>=<value> -t <imagename> .`: pass variables into the dockerfile using the `ARG` directive
- `docker build --platform <platform> -t <imagename> .`: build an image for a specific system architecture, such as `linux/amd64` or `linux/arm64`
- `docker build --pull -t <imagename> .`: pull the lastest base image from the registry
- `docker build --progress=plain -t <imagename> .`: show detailed logs during the build process
- `docker build -t <imagename> <pathtocontext>`: specify the directory containing the dockerfile and related files
- `docker build --target <stagename> -t <imagename> .`: build up to a specific stage in a multi-stage dockerfile
- `DOCKER_BUILDKIT=1 docker build -t registry.fci.vn/fptai-nlp/llama-factory:1.0.1 -f Dockerfile .`: BUILDKIT, build technology from docker, DOCKER_BUILD=1 mean creating a swap partition to build on machine 1
