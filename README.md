# Deep-Learning-Workstation-Setup
<a rel="license" href="http://creativecommons.org/licenses/by/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by/4.0/80x15.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by/4.0/">Creative Commons Attribution 4.0 International License</a>.

I am sharing my experiences setting up my Deep Learning Workstation. Feel free to contribute.

## Why am I doing this?
I am curious about where Machine Learning and Deep Learning and want to gather experience in applying it. Until now I completed some MOOCs which are listed in my [Class Central profile](https://www.classcentral.com/u/1246063).

## Buying the hardware
I got a good deal on a used [Lenovo ThinkStation P520 (30BE006X**)](https://psref.lenovo.com/Detail/ThinkStation/ThinkStation_P520?M=30BE006XGE) equiped as follows:
  * Xeon W-2133 6C/3.6GHz/8.25MB/140W/DDR4-2666
  * 900W Platinum Power Supply
  * 1x 32GB RDIMM DDR4-2666 ECC
  * 256GB SSD + 1 TB HDD, both SATA
  * 1x NVIDIA Quadro P4000 8GB (CUDA compute capability 6.1)

Applied modifications:
* The preinstalled 1TB HDD was replaced by a 1TB SSD (SATA).
* ToDo: Installation of an additional 512GB SSD (NVMe)

# Overview
Advantages of using Docker for a Deep Learning Workstation are that you will only need to install the following on your host system. 
* Linux OS
* Docker CE
* NVIDIA Driver
Everything else will be installed in the Docker container. Using containers you can make usage of different versions of CUDA at the same time in different containers. Therefore I believe using containers is the best way in developing Deep Learning models.

## [Installation instructions](InstallationInstructions.md)
This sections provides information on:
* Installing Ubuntu Server
* Enabling remote access via SSH
* Installing the NVIDIA driver and prior blacklisting of the Nouveau driver
* Installing Docker CE

## Docker for beginners
tbd

## Dockerized TensorFlow for beginners
tbd

## Screen for beginners
tbd

