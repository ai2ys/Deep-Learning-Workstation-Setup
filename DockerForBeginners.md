# Docker  for beginners

When I was new to Docker and was tired of repeatedly searching the internet for docker commands. Therefore I created this guide of my commonly used commands.

Link to the Docker CLI: [https://docs.docker.com/engine/reference/commandline/docker/]( https://docs.docker.com/engine/reference/commandline/docker/)

---

## List all Docker images

Listing all pulled and/or build images on your system using `docker image ls` or for short `docker images`.

```shell
$ docker image ls
REPOSITORY                                                  TAG                       IMAGE ID       CREATED        SIZE
ai2ys/deep-learning-workstation-setup/examples/tensorflow   22.10-tf2-py3             ac06a1248d15   14 hours ago   14.6GB
ai2ys/deep-learning-workstation-setup/examples/pytorch      22.10-py3                 d038ce706291   14 hours ago   17GB
nvidia/cuda                                                 12.0.1-base-ubuntu22.04   cdd8fce1e7b1   4 weeks ago    243MB
nvcr.io/nvidia/tensorflow                                   22.10-tf2-py3             6165f4ee35c6   4 months ago   14.4GB
nvcr.io/nvidia/pytorch                                      22.10-py3                 b1309fc25c28   4 months ago   16.9GB
```

When building images you should call `docker image prune` from time to time to remove dangling images.

## Start container with GPU support

This requires having installed the NVIDIA Container Toolkit. Be sure you have followed all instructions mentioned in [NVIDIA Container Toolkit instruction](InstallationInstructions.md#installing-the-nvidia-container-toolkit) previously.

GPUs can get made accessible using the option `--gpus all` for making all installed GPU accessible. Alternatively dedicated GPU can get made accessible using `--gpus <comma separated list of indices or GPU-UUIDS>`. Below an example passing the GPU with index `0` and running the command `nvidia-smi`.

```shell
$ docker run --rm --gpus 0  nvidia/cuda:12.0.1-base-ubuntu22.04 nvidia-smi
Sun Mar  5 10:18:51 2023       
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 525.85.12    Driver Version: 525.85.12    CUDA Version: 12.0     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|   0  NVIDIA TITAN RTX    On   | 00000000:65:00.0  On |                  N/A |
| 40%   40C    P0    56W / 280W |   1200MiB / 24576MiB |      2%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+
                                                                               
+-----------------------------------------------------------------------------+
| Processes:                                                                  |
|  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
|        ID   ID                                                   Usage      |
|=============================================================================|
+-----------------------------------------------------------------------------+
```

## Retrieve the IDs and names of containers

Use the command `docker ps` to list running containers. When additionally using the option `--all` also stopped container container will be displayed.

```shell
$ docker ps
CONTAINER ID   IMAGE                                     COMMAND                  CREATED              STATUS              PORTS                          NAMES
e40ed8584543   nvcr.io/nvidia/tensorflow:22.10-tf2-py3   "/opt/nvidia/nvidia_…"   About a minute ago   Up About a minute   6006/tcp, 6064/tcp, 8888/tcp   relaxed_hodgkin
```

An alternate way is to use the command `docker container ls`.

## Attach terminal to a running container

```shell
docker attach <container name or ID>
```

## Start a bash shell inside of a running container

```shell
docker exec -it <container name or ID> bash
```

## Docker and Jupyter notebooks

Assuming having started a Jupyter notebook in a container, but the terminal window being detached from the container (option `--detach`)

```shell
$ docker run --rm --detach -it --gpus all -p 8888:8888 nvcr.io/nvidia/tensorflow:22.10-tf2-py3 jupyter notebook --ip=0.0.0.0 --port=8888 --no-browser
0da83916789f149011098ad00cd61a7a7f297cf58e041eee457110cb6ec23c1
```

### Retrieve the token of a running dockerized Jupyter notebook

1. List all running docker containers

  ```shell
  $ docker ps
  CONTAINER ID   IMAGE                                     COMMAND                  CREATED         STATUS         PORTS                                                           NAMES
  0da83916789f   nvcr.io/nvidia/tensorflow:22.10-tf2-py3   "/opt/nvidia/nvidia_…"   2 minutes ago   Up 2 minutes   6006/tcp, 6064/tcp, 0.0.0.0:8888->8888/tcp, :::8888->8888/tcp   dazzling_benz
  ```

1. Access the token/link

  ```shell
  $ docker exec dazzling_benz jupyter notebook list
  Currently running servers:
  http://0.0.0.0:8888/?token=f20fd1fd97882e8bb9f211996cbae2b5227099a06ce58154 :: /workspace
  ```
  
  If JupyterLab is used instead of Jupyter notebook use `juypter lab list` instead of `jupyter notebook list`


## Using `docker compose`

Please checkout on how to use the `docker compose` following the examples mentiond in the main [README.md - Docker Compose examples with GPU support](README.md#docker-compose-examples-with-gpu-support) section.

