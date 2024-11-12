# A. Prepare a Dockerfile

```Dockerfile
FROM nvcr.io/nvidia/pytorch:23.12-py3

ENV LC_ALL=C.UTF-8 LANG=C.UTF-8

WORKDIR /nlp/

RUN apt-get update && \
    apt-get install -y tmux && \
    rm -rf /var/lib/apt/lists/*

COPY requirements.txt /nlp/requirements.txt

RUN pip install -U pip && \
    pip install -r requirements.txt

RUN MAX_JOBS=16 pip install flash-attn==2.3.3 --no-build-isolation
RUN pip install --upgrade nvidia-nccl-cu12==2.19.3

RUN pip install deepspeed
```

# B. Prepare a docker-compose.yml

# C. docker-compose up
