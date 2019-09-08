# OS installation

Although I am used to Windows and a newcomer in using Linux, I decided to go with Linux ([Ubuntu Server 18.04 LTS](http://releases.ubuntu.com/18.04/)) because I want to make use of Docker containers for Machine Learning (ML) and Deep Learning (DL) model development. Using Linux and Docker slowed down my progress a lot at the beginning, but I believe that I will gain speed later. Furthermore I am sure this setup will reduce the propability of me damaging the OS. At work I have been using Ubuntu Desktop for a while. As a result of this I decided to go with Ubuntu Server.

---
## Installing Ubuntu Server 18.04 LTS
Steps accomplished
* Preparation of a bootable USB stick with [Ubuntu Server 18.04 LTS](http://releases.ubuntu.com/18.04/)
* Installaltion of Ubuntu Server with default values
* Updating the system is up to date
  ```bash
  $ sudo apt-get update
  $ sudo apt-get upgrade
  ```
---
## Static network configuration for remote access
In order to access the workstation from a remote PC I configured the IP address to be static, following the instruction from [How to Configure Network Static IP Address in Ubuntu 18.04](https://www.tecmint.com/configure-network-static-ip-address-in-ubuntu/)
---
## Remote access from Windows laptop
I want to remotely access the workstation via SSH from my Windows system (ThinkPad Yoga 380 laptop with Windows 10 installed). 

Steps accomplished on the Ubuntu Server Workstation
* Check if the SSH service is active and start the service if needed
  ```bash
  $ service sshd status
  $ service sshd start
  ```
Steps accomplished on the Windows System (Laptop)
* Install [Putty](https://www.putty.org/)
* Start Putty Desktop App and save a session
  * Enter session name for "Saved Sessions"
  * Enter IP address of workstation
  * "Save" the session
* "Load" the required session and "Open" it
  * Now the login from Windows to the workstation is possible

---
## File transfer over SSH
When downloading files on the Windows system they can be transferred from the Windows system to the Linux system. Use PSCP from the Windows command shell by typing the following command.
* `<linux user>` &rarr; user name on Linux server
* `<linux pc-name>` &rarr; PC name of the Linux server
* `<destination directory>` &rarr; the destination directory on the Linux system
* `<source filepath on windows>` &rarr; the source file path on the Windows system
```bash
> pscp <source filepath on windows> <linux user>@<linux pc-name>:/home/<linux user>/<destination directory>/
```
---
## Installation of the NVIDIA GPU driver
__Remark:__ *At first I installed the NVIDIA driver from the NVIDIA website. But from time to time I faced some troubles because the driver was not working anymore. Threrefore I am now using the driver from the PPA repositiory. Until now it  is working.*

### Disabling Nouveau
Before installing the NVIDIA driver the Nouveau driver must be first disabled. [Instructions used for disabling the Nouveau driver.](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html#runfile-nouveau-ubuntu)

For Ubunutu create a file at `/etc/modprobe.d/blacklist-nouveau.conf`:
```bash
$ nano /etc/modprobe.d/blacklist-nouveau.conf
```
Add the following content and save the file.
```bash
blacklist nouveau
options nouveau modeset=0
```
Check the file content using `cat`:
```bash
$ cat /etc/modprobe.d/blacklist-nouveau.conf
```
Regenerate the kernel initramfs:
```bash
$ sudo update-initramfs -u
```

### Installation using the ppa repository
Deinstall the existing version.
```bash
$ dpkg -l | grep ndida
$ sudo apt-get purge *nvidia*
$ sudo autoremove
$ sudo autoclean
```

Steps accomplished for installing the driver version 435.
```bash
$ sudo add-apt-repository ppa:graphics-drivers
$ sudo apt-get update
# sudo apt-get install nvidia-driver-<version>, example for version 435.
$ sudo apt-get install nvidia-driver-435
$ sudo reboot
```
After the reboot call
```bash
$ nvidia-smi
```
I got the following output
```bash
Sun Sep  8 22:47:54 2019       
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 435.21       Driver Version: 435.21       CUDA Version: 10.1     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  TITAN RTX           Off  | 00000000:65:00.0  On |                  N/A |
| 41%   34C    P8     5W / 280W |   1185MiB / 24219MiB |      2%      Default |
+-------------------------------+----------------------+----------------------+
                                                                               
+-----------------------------------------------------------------------------+
| Processes:                                                       GPU Memory |
|  GPU       PID   Type   Process name                             Usage      |
|=============================================================================|
|    0      2048      G   /usr/lib/xorg/Xorg                            40MiB |
|    0      2248      G   /usr/bin/gnome-shell                          51MiB |
|    0      2689      G   /usr/lib/xorg/Xorg                           448MiB |
|    0      2842      G   /usr/bin/gnome-shell                          93MiB |
|    0      3423      G   ...uest-channel-token=13807832972120768915   461MiB |
|    0      3892      G   ...quest-channel-token=8347145569574175397    87MiB |
+-----------------------------------------------------------------------------+
```

---
## Installing Docker CE
Follow the instruction from [Get Docker CE for Ubuntu](https://docs.docker.com/install/linux/docker-ce/ubuntu/).

I decided to install the debian package manually (Docker v18.06.0 for bionic). I got an error message that a dependency was missing and had to install ```libltdl7``` previously.
<br>
Edit: Recently, I updated Docker to version 19.03.2.

Additionally I added my user to the docker user group as decribed here:<br>
[Manage Docker as a non-root user](https://docs.docker.com/install/linux/linux-postinstall/)

After rebooting try to run the ```hello-world``` docker container. If it runs everything is working.

Ensure that docker will start on boot. 
```bash
$ service --status-all
$ systemctl is-enabled docker
$ systemctl enable docker
```

---
## Installing the NVIDIA Container Toolkit
The previous nvidia-docker2 is now deprecated. Therefore I have to deinstall the previous isntalled verion.
```bash
$ sudo apt-get purge -y nvidia-docker
```
Afterwards follow the instruction from [nvidia-docker on GitHub](https://github.com/NVIDIA/nvidia-docker).

```bash
# Add the package repositories
$ distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
$ curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
$ curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list

$ sudo apt-get update && sudo apt-get install -y nvidia-container-toolkit
$ sudo systemctl restart docker
``` 
The following command did run successfully.
```bash
#### Test nvidia-smi with the latest official CUDA image
$ docker run --gpus all nvidia/cuda:latest nvidia-smi
```
It did work and I got the following output.
```bash
Sun Sep  8 20:45:05 2019       
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 435.21       Driver Version: 435.21       CUDA Version: 10.1     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  TITAN RTX           Off  | 00000000:65:00.0  On |                  N/A |
| 41%   34C    P8     5W / 280W |   1203MiB / 24219MiB |      6%      Default |
+-------------------------------+----------------------+----------------------+
                                                                               
+-----------------------------------------------------------------------------+
| Processes:                                                       GPU Memory |
|  GPU       PID   Type   Process name                             Usage      |
|=============================================================================|
+-----------------------------------------------------------------------------+

```
