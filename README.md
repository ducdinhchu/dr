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
### 4. Docker Run Commands
- `docker run -d <imagename>`: run a docker images as a container in detached mode
- `docker run -p <hostport>:<containerport> <imagename>`: publish container ports to the host
- `docker run -v <hostpath>:<containerpath> <imagename>`: mount the host directory or volume to the container
- `docker run --name <containername> <imagename>`: assign a name to the container
### 5. Docker Clean Up Commands
- `docker system prune`: remove all unused docker resources, including containers, images, networks, and volumns
- `docker container prune`: remove all stopped containers
- `docker image prune`: remove unused images
- `docker volumn prune`: remove unused volumns
- `docker network prune`: remove unused networks
### 6. Image Commands
- `docker images`: list available docker images
- `docker pull <imagename>`: pull a docker image from a docker registry
- `docker push <imagename>`: push a docker image to a docker registry
- `docker rmi <imageid>`: remove a docker image
### 7. Docker Env Variables
- `-e` or `--env`: set env variables when running a container
- `docker run -e <varname>=<varvalue> <imagename>`: set an env variable when running a container
### 8. Docker Compose Commands
- `docker-compose up`: create and start containers defined in a docker compose file
- `docker-compose down`: stop and remove containers defined in a docker compose file
- `docker-compose ps`: list containers defined in a docker compose file
- `docker-compose log`: view logs of containers defined in a docker compose file
