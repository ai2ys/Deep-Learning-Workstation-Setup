# Deep-Learning-Workstation-Setup
<a rel="license" href="http://creativecommons.org/licenses/by/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by/4.0/80x15.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by/4.0/">Creative Commons Attribution 4.0 International License</a>.

I am sharing my experiences setting up my Deep Learning Workstation. The main reason I am doing this documentation is to be able to redo all of the installation and configuring if needed. Furthermore, I hope this documentation helps other who are getting started with this topic. Since I am a beginner in this field I might not have used the best way for accomplishing my goal. Feel free to contribute.

## Why am I doing this?
I am curious about where Machine Learning and Deep Learning and want to gather experience in applying it. Until now I completed some MOOCs which are listed in my [Class Central profile](https://www.classcentral.com/u/1246063).

## Buying the hardware
I got a good deal on a used [Lenovo ThinkStation P520 (30BE006X**)](https://psref.lenovo.com/Detail/ThinkStation/ThinkStation_P520?M=30BE006XGE) equiped as follows:
  * Xeon W-2133 6C/3.6GHz/8.25MB/140W/DDR4-2666
  * 900W Platinum Power Supply
  * 1x 32GB RDIMM DDR4-2666 ECC
  * 256GB SSD + 1 TB HDD, both SATA
  * 1x NVIDIA Quadro P4000
    ([Data sheet](https://www.nvidia.com/content/dam/en-zz/Solutions/design-visualization/productspage/quadro/quadro-desktop/quadro-pascal-p4000-data-sheet-us-nvidia-704358-r2-web.pdf))
    * 1792 CUDA cores 
    * 8 GB GDDR5 GPU Memory
    * CUDA compute capability 6.1    
    * 5.3 TFLOPS FP32 performance

Applied modifications:
* The preinstalled 1TB HDD was replaced by a 1TB SSD (SATA).
* ToDo: Installation of an additional 512GB SSD (NVMe)

# Overview
Advantages of using Docker for a Deep Learning Workstation are that you will only need to install the following on your host system. 
* Linux OS
* Docker CE
* NVIDIA Driver

Everything else will be installed in the Docker container. Using containers you can make usage of different versions of CUDA at the same time in different containers. Therefore I believe using them is the best way in developing Deep Learning models.

![Image from nvida-docker](https://cloud.githubusercontent.com/assets/3028125/12213714/5b208976-b632-11e5-8406-38d379ec46aa.png)<br>
(source: https://github.com/NVIDIA/nvidia-docker)

## [Installation instructions](./InstallationInstructions.md)
The (InstallationInstruction.md) file provides information on:
* [Installing Ubuntu Server](./Deep-Learning-Workstation-Setup.md#Installing-Ubuntu-Server-1804-LTS)
* [Enabling remote access via SSH](./InstallationInstructions.md#remote-access-from-windows-laptop)
* [Installing the NVIDIA driver and prior blacklisting of the Nouveau driver](./InstallationInstructions.md#installation-of-the-nvidia-gpu-driver)
* [Installing Docker CE](InstallationInstructions.md#installing-docker-ce)

## [Docker for beginners](./DockerForBeginners.md)
This (DockerForBeginners.md) files provides information on some of my commonly used docker commands, e.g.:
* List running docker containers
* [Retrieve the token of a running dockerized Jupyter notebook instance](./DockerForBeginners.md#retrieving-the-token-of-a-running-dockerized-jupyter-notebook-instance)

## Dockerized TensorFlow for beginners
tbd

## Screen for beginners
tbd

