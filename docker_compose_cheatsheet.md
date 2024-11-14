### Common commands
```bash
# start existing containers for a service
docker-compose start

# stop running containers without removing them
docker-compose stop

# pause running containers of a service
docker-compose pause

# unpause paused containers of a service
docker-compose unpause

# list containers
docker-compose ps

# build, (re)create, start, and attach to containers for a service
docker-compose up

# stop and remove containers, networks, volumes, and images created by up
docker-compose down
```
### docker-compose.yml
```yml
services:
  llamafactory:
    build:
      dockerfile: ./docker/docker-cuda/Dockerfile
      context: ../..
      args:
        INSTALL_BNB: false
        INSTALL_VLLM: false
        INSTALL_DEEPSPEED: false
        INSTALL_FLASHATTN: false
        INSTALL_LIGER_KERNEL: false
        INSTALL_HQQ: false
        INSTALL_EETQ: false
        PIP_INDEX: https://pypi.org/simple
    container_name: llamafactory
    volumes:
      - ../../hf_cache:/root/.cache/huggingface
      - ../../ms_cache:/root/.cache/modelscope
      - ../../om_cache:/root/.cache/openmind
      - ../../data:/app/data
      - ../../output:/app/output
    ports:
      - "7860:7860"
      - "8000:8000"
    ipc: host
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
    restart: unless-stopped
```
