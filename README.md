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
### 3. Docker Run Commands
- `docker run -d <imagename>`: run a docker image as a container in detached mode
- `docker run -p <hostport>:<containerport> <imagename>`: publish container ports to the host
- `docker run -v <hostpath>:<containerpath> <imagename>`: mount the host directory or volume to the container
- `docker run --name <containername> <imagename>`: assign a name to the container
### 4. Container Inspection Commands
- `docker ps`: list running containers
- `docker ps -a`: list all containers, including stopped ones
- `docker logs <containerid>`: fetch the logs of a specified container
- `docker inspect <containerid>`: inspect detailed information about a container
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

# Dockerfile
```dockerfile
# Activate Docker BuildKit, which enhances build performance and provides advanced features
# DOCKER_BUILDKIT=1 docker build -t registry.fci.vn/fptai-nlp/llama-factory:1.0.1 -f Dockerfile .

# The `FROM` instruction specifies the base image for subsequent instructions. This Dockerfile uses NVIDIA's PyTorch image from the NVIDIA NGC (NVIDIA GPU Cloud) registry,
# which is optimized for GPU-accelerated deep learning applications.
From nvcr.io/nvidia/pytorch:23.12-py3

# Set env variables
ENV LC_ALL=C.UTF-8 LANG=C.UTF-8

# Set working directory
WORKDIR /nlp/

# Install tmux and clean up
RUN apt-get update && \
    apt-get install -y tmux && \
    rm -rf /var/lib/apt/lists/*

# Copy requirements.txt file into container
COPY requirements.txt /nlp/requirements.txt

# Upgrade pip and install packages specified in requirements.txt
RUN pip install -U pip \
&& pip install -r requirements.txt

# Install flash-attn and upgrade nvidia-nccl-cu12
RUN MAX_JOBS=16 pip install flash-attn==2.3.3 --no-build-isolation
RUN pip install --upgrade nvidia-nccl-cu12==2.19.3

# Install deepspeed
RUN pip install deepspeed
```

# docker-compose.yml
```yml
services:
  llamafactory:
    build:
      dockerfile: ./docker/docker-cuda/Dockerfile # Dockerfile path
      context: ../.. # context path
      args:
        INSTALL_BNB: false # arg to control installation of BNB
        INSTALL_VLLM: false
        INSTALL_DEEPSPEED: false
    container_name: llamafactory # container name
    volumes:
      - ../../hf_cache:/root/.cache/huggingface # mount host's dir to container's dir
      - ../../ms_cache:/root/.cache/modelscope
      - ../../om_cache:/root/.cache/openmind
    ports:
      - "7860:7860" # map host's port to container's port
      - "8000:8000"
    ipc: host # inter-process communication, configure container to use host's IPC namspace instead of an isolated one, processes inside container can communicate with processes on host and other containers can share host IPC namespace
    tty: true
    stdin_open: true
    command: bash
    deploy:
      resources:
        reservations:
          devices:
          - driver: nvidia
            count: "all"
            capabilities: [gpu]
    restart: unless-stopped # restart container unless explicitly stopped
```
