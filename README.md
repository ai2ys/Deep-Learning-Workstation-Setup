# Deep-Learning-Workstation-Setup

<a rel="license" href="http://creativecommons.org/licenses/by/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by/4.0/80x15.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by/4.0/">Creative Commons Attribution 4.0 International License</a>.

I am sharing my experiences setting up my Deep Learning Workstation. The main reason I am doing this documentation is to be able to redo all of the installation and configuring if needed. Furthermore, I hope this documentation helps others who are getting started with this topic. Feel free to contribute.

---

## Overview

Advantages of using Docker for a Deep Learning Workstation are that you will only need to install the following on your host system.

- Linux OS (Ubuntu 20.04)
- Docker CE
- NVIDIA Driver

Everything else will be installed in the Docker container. Using containers you can make usage of different versions of CUDA at the same time in different containers. Therefore I believe using them is the best way in developing Deep Learning models.

![Image from nvida-docker](https://cloud.githubusercontent.com/assets/3028125/12213714/5b208976-b632-11e5-8406-38d379ec46aa.png)\
(source: https://github.com/NVIDIA/nvidia-docker)

---

## Installation instructions

The [InstallationInstructions.md](./InstallationInstructions.md) file provides information on:

- [Installing Ubuntu Server](./Deep-Learning-Workstation-Setup.md#Installing-Ubuntu-Server-1804-LTS)
- [Enabling remote access via SSH](./InstallationInstructions.md#remote-access-from-windows-laptop)
- [Installing the NVIDIA driver and prior blacklisting of the Nouveau driver](./InstallationInstructions.md#installation-of-the-nvidia-gpu-driver)
- [Installing Docker CE](InstallationInstructions.md#installing-docker-ce)
    \
    After having installed Docker, checkout the following sections below
  - [Docker for beginners](#docker-for-beginners)
  - [`docker compose` examples with GPU support](#docker-compose-examples-with-gpu-support)
      \
      Here you will find also examples for PyTorch and TensorFlow

### Docker for beginners

This [DockerForBeginners.md](./DockerForBeginners.md) files provides information on some of my commonly used docker commands, e.g.:

- List running docker containers
- [Retrieve the token of a running dockerized Jupyter notebook instance](./DockerForBeginners.md#retrieving-the-token-of-a-running-dockerized-jupyter-notebook-instance)

### `docker compose` examples with GPU support

The folder [./examples](./examples/) contains multiple examples for running Docker containers with GPU support.

1. __nvidia-smi__: [examples/nvidia-cuda/README.md](examples/nvidia-cuda/README.md)
    \
    Start with this example which uses a pre-built image with `docker compose` indicating how a GPU can get made accessible within a container using `docker compose`.
    Afterwards try out the PyTorch or TensorFlow examples
1. __TensorFlow__, __PyTorch__
    \
    After having tried out the example mentioned in 1.) try out this one which customized an image based on `nvcr.io/nvidia/pytorch` and `nvcr.io/nvidia/tensorflow` images.

    1. __PyTorch__ [examples/pytorch/README.md](examples/pytorch/README)
    1. __TensorFlow__ [examples/pytorch/README.md](examples/pytorch/README)

ℹ️ I personally prefer using `docker-compose.yml` files as they offer a clean way to build images and start containers without the need of long and tedious commands on the cli or the need for hard to maintain bash scripts.

---

## My hardware setup

In 2018 I got a good deal on a used [Lenovo ThinkStation P520 (30BE006X**)](https://psref.lenovo.com/Detail/ThinkStation/ThinkStation_P520?M=30BE006XGE) equiped as follows:

- Xeon W-2133 6C/3.6GHz/8.25MB/140W/DDR4-2666
- 900W Platinum Power Supply
- 1x 32GB RDIMM DDR4-2666 ECC
- 256GB SSD + 1 TB HDD, both SATA
- 1x NVIDIA Quadro P4000
  ([Data sheet](https://www.nvidia.com/content/dam/en-zz/Solutions/design-visualization/productspage/quadro/quadro-desktop/quadro-pascal-p4000-data-sheet-us-nvidia-704358-r2-web.pdf))
  - 1792 CUDA cores
  - 8 GB GDDR5 GPU Memory
  - CUDA compute capability 6.1
  - 5.3 TFLOPS FP32 performance

Modifications over time:

- Replacements
  - GPU: NVIDIA TITAN RTX replacing the NVIDIA Quadro P4000
  - SSD: 1TB SSD (SATA)replacing the 1TB HDD
  - RAM: 4x 32GB RAM (same type but different vendor) replacing the 1x 32GB RAM
- Added hardware
  - SSD: 512GB SSD (NVMe)