# Docker for beginners
I am new to docker and I am tired of repeatedly searching the internet for docker commands. Therefore I created this guide of my commonly used commands.

## Docker and Jupyter notebooks
## Retrieving the token of a running dockerized Jupyter notebook instance
* list all running docker containers
  ```bash
  $ docker container ls
  CONTAINER ID        IMAGE                                      COMMAND                  CREATED             STATUS              PORTS                    NAMES
  8f327fe4419d        ai2ys/tensorflow:2.0.0a0-gpu-py3-jupyter   "bash -c 'source /et…"   15 hours ago        Up 15 hours         0.0.0.0:8888->8888/tcp   romantic_lamarr
  ```
* access the token
  ```bash
  docker exec 8f327fe4419d jupyter notebook list 
  Currently running servers:
  http://0.0.0.0:8888/?token=6805c223892ec4e1017092f15cf1a7aa7807852c0f17d154 :: /tf
  ```
  
* copy paste the url into the brower and replace the `0.0.0.0` with the appropriate IP address
