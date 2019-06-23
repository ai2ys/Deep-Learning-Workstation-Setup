# Docker for beginners
I am new to docker and I am tired of repeatedly searching the internet for docker commands. Therefore I created this guide of my commonly used commands.

Link to the Docker CLI: https://docs.docker.com/engine/reference/commandline/docker/

## Retrieve the IDs and names containers
Use the command `docker ps` to list running containers. When additionally using the option `--all` also the not running container will be displayed.
```bash
$ docker ps
CONTAINER ID        IMAGE                                      COMMAND                  CREATED             STATUS              PORTS                                            NAMES
e0f33c4fd9b4        ai2ys/tensorflow:2.0.0a0-gpu-py3-jupyter   "bash -c 'source /et…"   About an hour ago   Up About an hour    0.0.0.0:6006->6006/tcp, 0.0.0.0:8888->8888/tcp   relaxed_chaplygin
```
An alternate way is to use the command `docker container ls`.

## Attach terminal to running container
```bash
$ docker attach <container name or ID>
```

## Start bash shell inside a runnning container
```bash
$ docker exec -it <container name or ID> bash
```

## Docker and Jupyter notebooks
### Retrieving the token of a running dockerized Jupyter notebook instance
* list all running docker containers
  ```bash
  $ docker container ls
  CONTAINER ID        IMAGE                                      COMMAND                  CREATED             STATUS              PORTS                    NAMES
  8f327fe4419d        ai2ys/tensorflow:2.0.0a0-gpu-py3-jupyter   "bash -c 'source /et…"   15 hours ago        Up 15 hours         0.0.0.0:8888->8888/tcp   romantic_lamarr
  ```
* access the token
  ```bash
  $ docker exec 8f327fe4419d jupyter notebook list 
  Currently running servers:
  http://0.0.0.0:8888/?token=6805c223892ec4e1017092f15cf1a7aa7807852c0f17d154 :: /tf
  ```
  
* copy paste the url into the brower and replace the `0.0.0.0` with the appropriate IP address
