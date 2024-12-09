# OS installation

The system is setup using Linux ([Ubuntu 20.04 LTS](http://releases.ubuntu.com/20.04/)) as this works best using (🐋 Docker) containers for Machine Learning (ML)/Deep Learning (DL) model development with NVIDIA GPUs.

---

## Installing Ubuntu 

In this example Ubuntu 20.04 LTS will be installed.

Steps accomplished:

- Preparation of a bootable USB stick with Ubuntu
  - here: [Ubuntu Server 20.04 LTS](http://releases.ubuntu.com/20.04/)
- Installaltion of Ubuntu Desktop (minimal)
- Updating the system is up to date
    ```markdown
    sudo apt-get update
    sudo apt-get upgrade
    ```

## Upgrading Ubuntu Version

Upgrade will only be possible from the current version to the next version, e.g. from 20.04 LTS to 22.04 LTS.

Following instructions from 
<https://www.digitalocean.com/community/tutorials/how-to-upgrade-to-ubuntu-22-04-jammy-jellyfish>.


```bash
sudo apt-get update
sudo apt-get upgrade
sudo reboot now

sudo apt-get dist-upgrade
sudo reboot now

sudo do-release-upgrade
# reboot will be done automatically within the upgrade process
```

Aftewards check the current version using `lsb_release -a`.

```bash
$ lsb_release -a
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 22.04.5 LTS
Release:        22.04
Codename:       jammy 
```

Additionally checking if NVIDIA driver is still installed and working properly using `nvidia-smi`.

```bash
$ nvidia-smi
Mon Dec  9 11:15:48 2024       
+-----------------------------------------------------------------------------------------+
| NVIDIA-SMI 550.127.08             Driver Version: 550.127.08     CUDA Version: 12.4     |
|-----------------------------------------+------------------------+----------------------+
| GPU  Name                 Persistence-M | Bus-Id          Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |           Memory-Usage | GPU-Util  Compute M. |
|                                         |                        |               MIG M. |
|=========================================+========================+======================|
|   0  NVIDIA TITAN RTX               Off |   00000000:65:00.0 Off |                  N/A |
| 41%   30C    P8              4W /  280W |      16MiB /  24576MiB |      0%      Default |
|                                         |                        |                  N/A |
+-----------------------------------------+------------------------+----------------------+
```

---
## Static network configuration for remote access

In order to access the workstation from a remote PC I configured the IP address to be static.
<!--
, following the instruction from [How to Configure Network Static IP Address in Ubuntu 18.04](https://www.tecmint.com/configure-network-static-ip-address-in-ubuntu/)
-->

---
## Remote access from Windows laptop

I want to remotely access the workstation via SSH from my Windows system (ThinkPad Yoga 380 laptop with Windows 10 installed). I will use VS Code in order to connect via SSH to the remote system as well as connecting it "attaching VS Code" to Docker containers running on the remote system.

Steps accomplished on the Ubuntu Server Workstation

- Install ssh service

  ```bash
  sudo apt-get update
  sudo apt-get install openssh-server
  ```

- Check if the SSH service is active and start the service if needed

  ```bash
  service sshd status
  service sshd start
  ```
  
<!--
Steps accomplished on the Windows System (Laptop)
* Install [Putty](https://www.putty.org/)
* Start Putty Desktop App and save a session
  * Enter session name for "Saved Sessions"
  * Enter IP address of workstation
  * "Save" the session
* "Load" the required session and "Open" it
  * Now the login from Windows to the workstation is possible
-->

Using SSH for login

- Create a SSH key on the local system, in case of Windows use GitBash for the following steps
      ```bash
      ssh-keygen -b4096 -t rsa -C "some comment"
      ```
- Append the public key (`*.pub`) to the file `~/.ssh/authorized_keys` on the remote system
  - Create the file in case it does not exist  
    ```bash
    touch .ssh/authorized_keys
    ```
  - Copy the public key `*.pub` and append it to the `authorized_keys`  
    ```bash
    # append the key (here id_rsa.pub) to the authorized keys
    cat id_rsa.pub >> ~./ssh/authorized_keys
    ```
  
Configure a SSH configuration file

```bash
# Read more about SSH config files: https://linux.die.net/man/5/ssh_config
Host <name_alias>
    HostName <ip address>
    User username
    PreferredAuthentications publickey
    # sample for Linux
    IdentityFile ~/.ssh/id_rsa
```

- Linux: `.ssh/config`
- Windows:
  - Standard: `C:\Users\<username>\.ssh\config`
  - WSL2: `/mnt/c/Users/<username>/.ssh/config`

---

## File transfer via SSH (Windows ➡️ Linux)

When downloading files on the Windows system they can be transferred from the Windows system to the Linux system via command line using `pscp`.

```bash
pscp <src filepath on windows> <linux user>@<linux pc-name>:/home/<linux user>/<destination directory>/
```

- `<linux user>` &rarr; user name on Linux server
- `<linux pc-name>` &rarr; PC name of the Linux server
- `<destination directory>` &rarr; the destination directory on the Linux system
- `<source filepath on windows>` &rarr; the source file path on the Windows system

ℹ️ Alternatively files and folder can be transferred "drag & drop" in VSCode form your host (MacOS, Linux, Windows) to the Linux server to the current opened folder on the Linux server.

---

## Installation of the NVIDIA GPU driver

### Disabling Nouveau
Before installing the NVIDIA driver the Nouveau driver must be first disabled. [Instructions used for disabling the Nouveau driver.](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html#runfile-nouveau-ubuntu)

- For Ubunutu create a file at `/etc/modprobe.d/blacklist-nouveau.conf`:

    ```bash
    nano /etc/modprobe.d/blacklist-nouveau.conf
    ```

    - Add the following content and save the file.
    
        ```bash
        blacklist nouveau
        options nouveau modeset=0
        ```

- Check the file content using `cat`:

    ```bash
    $ cat /etc/modprobe.d/blacklist-nouveau.conf
    blacklist nouveau
    options nouveau modeset=0
    ```

- Regenerate the kernel initramfs:

    ```bash
    sudo update-initramfs -u
    ```


### Installation using the ppa repository

Deinstallation of previously installed version.

- Retrieve installed NVIDIA driver version using

    ```shell
    dpkg -l | grep nvidia
    ```
    or
    ```shell
    whereis nvidia
    ```
    or
    ```shell
    modinfo nvidia | grep ^version
    ```

- Deinstall/remove all version related packages, replace the retrieved version below, e.g. 470

    ```shell
    current_version=470
    sudo apt-get purge *nvidia*${current_version}
    sudo apt-get autoremove
    sudo apt-get clean
    ```

Steps accomplished for installing the new driver version

- Add `ppa` repository, if not already done previously
  - Check if already added previously

    ```shell
    grep ^ /etc/apt/sources.list /etc/apt/sources.list.d/* | grep graphics-drivers
    ```

  - If `ppa` repository is not present ad it and update

    ``` shell
    sudo add-apt-repository ppa:graphics-drivers
    sudo apt-get update
    ```

- Check which major driver version is compatible with your NVIDIA GPU. [Retrieve compatible version](https://www.nvidia.de/Download/index.aspx), e.g. version "525.89.02" for NVIDIA RTX TITAN 2023-03-04
  
- Install new driver version, e.g. 525
  - Check for availability, install and reboot afterwards

    ```shell
    new_version=525
    apt-cache search nvidia-driver | grep ${new_version}
    sudo apt-get install nvidia-driver-${new_version}
    sudo reboot now
    ```

After the system has been rebooted you should get a similar output as below when running `nvidia-smi` from the command line.

``` shell
$ nvidia-smi
Sat Mar  4 12:56:05 2023
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 525.85.12    Driver Version: 525.85.12    CUDA Version: 12.0     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|   0  NVIDIA TITAN RTX    On   | 00000000:65:00.0  On |                  N/A |
| 41%   40C    P0    56W / 280W |   1416MiB / 24576MiB |      3%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+
```

### Update NVIDIA driver installed via pps repository

As already mentioned above, check which major driver version is compatible with your NVIDIA GPU. [Retrieve compatible version](https://www.nvidia.de/Download/index.aspx)

```shell
# retrieve installed NVIDIA driver version using
dpkg -l | grep nvidia
# or
whereis nvidia
# or
modinfo nvidia | grep ^version

# replace the retrieved version below
current_version=<version>
# define version that should get installed
new_version=<new_version>
# check availability of driver version
apt-cache search nvidia-driver | grep ${new_version}
# deinstall/remove all version related packages
sudo apt-get purge *nvidia*${current_version}
sudo apt-get autoremove
sudo apt-get clean
# install version new version
sudo apt-get install nvidia-driver-${new_version}
# reboot
sudo reboot now

# after reboot check
nvidia-smi
```

---
## Installing Docker CE

Follow the instruction from [Get Docker CE for Ubuntu](https://docs.docker.com/install/linux/docker-ce/ubuntu/).

I decided to install the debian package manually (Docker v18.06.0 for bionic). I got an error message that a dependency was missing and had to install ```libltdl7``` previously.
\
Edit: Recently, I updated Docker to version 23.0.1, build a5ee5b1.

Additionally I added my user to the docker user group as decribed here:\
[Manage Docker as a non-root user](https://docs.docker.com/install/linux/linux-postinstall/)

After rebooting try to run the ```hello-world``` docker container. If it runs everything is working.

Ensure that docker runs as system-wide service and will start on boot.

```shell
service --status-all
systemctl is-enabled docker
systemctl enable docker
```

---

### Installing the NVIDIA Container Toolkit

The previous nvidia-docker2 is now deprecated. Therefore I have to uninstall the previous installed version.

```bash
sudo apt-get purge -y nvidia-docker
```

Afterwards follow the instruction from [nvidia-docker on GitHub](https://github.com/NVIDIA/nvidia-docker).

```shell
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -

curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list

sudo apt-get update && sudo apt-get install -y nvidia-container-toolkit
sudo systemctl restart docker
```

Afterwards try out running an `nvidia/cuda` container and executing the command `nvidia-smi` in the container as done with the command `docker run --rm --gpus all nvidia/cuda:12.0.1-base-ubuntu22.04 nvidia-smi`. It will make all the installed gpus available within the container.

```shell
$ docker run --rm --gpus all nvidia/cuda:12.0.1-base-ubuntu22.04 nvidia-smi
Sat Mar  4 14:18:15 2023       
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 525.85.12    Driver Version: 525.85.12    CUDA Version: 12.0     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|   0  NVIDIA TITAN RTX    On   | 00000000:65:00.0  On |                  N/A |
| 41%   37C    P0    56W / 280W |   1537MiB / 24576MiB |      8%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+
```

ℹ️ If you like specific GPUs being available in the container you can either specify them by their index or GPU-UUID. The indices as well as the UUIDs can get retrieved using `nvidia-smi --list-gpus`.

```shell
$ nvidia-smi --list-gpus
GPU 0: NVIDIA TITAN RTX (UUID: GPU-<UUID removed here>)
```

---

### Running Docker rootless mode

After the installation of Docker the service will run as system-wide service. All users on the system will be able to access all images/containers available/running on the system. This can be an issue when sharing a workstation with multiple users.

It is possible to install Docker as a service per user. Each user will only see its own images and containers.

ℹ️ I changed my setup to Docker rootless mode. For information on how this as accomplished see [DockerRootless.md](DockerRootless.md).

---

### Installing the `docker compose` plugin
Using `docker-compose.yml` files for building the Docker images and running the containers makes life much easier. It is mainly used for multi-container applications, but I find it also very useful for running single containers, see [Docker Compose overview](https://docs.docker.com/compose/).

For using the `docker-compose.yml` files you have to install the `docker compose` plugin. Follow the instructions from [Installation of the Compose plugin](https://docs.docker.com/compose/install/linux/) for Ubuntu as shown below.

```shell
sudo apt-get update
sudo apt-get install docker-compose-plugin
```

After the installation check `docker compose version`. You should get an output similar to the one below.

```shell
$ docker compose version
Docker Compose version v2.16.0
```

Please checkout on how to use the `docker compose` following the examples mentiond in the main [README.md - Docker Compose examples with GPU support](README.md#docker-compose-examples-with-gpu-support) section.
