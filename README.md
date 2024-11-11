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
### 2. Container Interaction Commands
- `docker run <imagename>`: run a docker image as a container
- `docker start <containerid>`: start a stopped container
- `docker stop <containerid>`: stop a running container
- `docker restart <containerid>`: restart a running container
- `docker exec -it <containerid> <command>`: execute a command inside a running container interactively
### 3. Container Inspection Commands
- `docker ps`: list running containers
- `docker ps -a`: list all containers, including stopped ones
- `docker logs <containerid>`: fetch the logs of a specified container
- `docker inspect <containerid>`: inspect detailed information about a container
### 3. Docker Clean Up Commands 
