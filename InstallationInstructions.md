## OS installation
Although I am used to Windows and a newcomer in using Linux, I decided to go with Linux ([Ubuntu Server 18.04 LTS](http://releases.ubuntu.com/18.04/)) because I want to make use of Docker containers for Machine Learning (ML) and Deep Learning (DL) model development. Using Linux and Docker slowed down my progress a lot at the beginning, but I believe that I will gain speed later. Furthermore I am sure this setup will reduce the propability of me damaging the OS. At work I have been using Ubuntu Desktop for a while. As a result of this I decided to go with Ubuntu Server.

### Installing Ubuntu Server 18.04 LTS
Steps accomplished
* Preparation of a bootable USB stick with [Ubuntu Server 18.04 LTS](http://releases.ubuntu.com/18.04/)
* Installaltion of Ubuntu Server with default values
* Updating the system is up to date
  ```bash
  $ sudo apt-get update
  $ sudo apt-get upgrade
  ```
### Static network configuration for remote access
In order to access the workstation from a remote PC I configured the IP address to be static, following the instruction from [How to Configure Network Static IP Address in Ubuntu 18.04](https://www.tecmint.com/configure-network-static-ip-address-in-ubuntu/)

### Remote access from Windows laptop
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

### File transfer over SSH
When downloading files on the Windows system they can be transferred from the Windows system to the Linux system. Use PSCP from the Windows command shell by typing the following command.
* `<linux user>` &rarr; user name on Linux server
* `<linux pc-name>` &rarr; PC name of the Linux server
* `<destination directory>` &rarr; the destination directory on the Linux system
* `<source filepath on windows>` &rarr; the source file path on the Windows system
```bash
> pscp <source filepath on windows> <linux user>@<linux pc-name>:/home/<linux user>/<destination directory>/
```

### Installation of the NVIDIA GPU driver
Before installing the NVIDIA driver the Nouveau driver must be first disabled. 

#### Disabling Nouveau
[Instructions used for disabling the Nouveau driver.](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html#runfile-nouveau-ubuntu)
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

#### Download and installation
In order to download the appropriate NVIDIA GPU driver from the [NVIDIA Driver Downloads](https://www.nvidia.com/Download/index.aspx?lang=en-us) site information about the GPU must be provided.
* Product type (TITAN, GeForce, Quadro, ...)
* Product series
* OS
* etc. ...

I downloaded version 418 on the Windows system and transferred the file to the Linux workstation using PSCP.
```bash
> pscp NVIDIA-Linux-x86_64-418.56.run <linux user>@<linux pc-name>:/home/<linux user>
```

The installation was accomplished following the instructions from [NVIDIA Linux-x86_64 418.56](http://us.download.nvidia.com/XFree86/Linux-x86_64/418.56/README/index.html) section 4 [Installing the NVIDIA Driver](http://us.download.nvidia.com/XFree86/Linux-x86_64/418.56/README/installdriver.html).

## Installing Docker CE
Follow the instruction from [Get Docker CE for Ubuntu](https://docs.docker.com/install/linux/docker-ce/ubuntu/).

I decided to install the debian package manually (Docker v18.06.0 for bionic). I got an error message that a dependency was missing and had to install ```libltdl7``` previously.
<br>
Edit: Recently, I updated Docker to version 18.09.5.

Additionally I added my user to the docker user group as decribed here:<br>
[Manage Docker as a non-root user](https://docs.docker.com/install/linux/linux-postinstall/)

After rebooting try to run the ```hello-world``` docker container. If it runs everything is working.

Ensure that docker will start on boot. 
```bash
$ services --status-all
$ systemctl is-enabled docker
$ systemctl enable docker
```

## Installing nvidia-docker2
Follow the instruction from [nvidia-docker on GitHub](https://github.com/NVIDIA/nvidia-docker)
* [Instruction for Ubuntu](https://github.com/NVIDIA/nvidia-docker#ubuntu-140416041804-debian-jessiestretch)
