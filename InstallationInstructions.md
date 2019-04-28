## OS installation
Although I am used to Windows and newcomer in using Linux, I decided to go with Linux ([Ubuntu Server 18.04 LTS](http://releases.ubuntu.com/18.04/)) because I want to use Docker containers for development. Using Linux and Docker slowed my progress a lot at the beginning, but I believe that I will gain speed later and I this setup will reduce the propability of me damaging the OS. At work I have been using Ubuntu Desktop for a while. As a result of this I chose to go with Ubuntu Server.

### Installing Ubuntu Server 18.04 LTS
Steps acomplished
* Prepare a bootable USB stick with [Ubuntu Server 18.04 LTS](http://releases.ubuntu.com/18.04/)
* Install Ubuntu Server with default values
* Ensure the system is up to date
  ```bash
  $ sudo apt-get update
  $ sudo apt-get upgrade
  ```
### Remote access from Windows laptop
I want to remotely access the workstation via SSH from my Windows system (ThinkPad Yoga 380 laptop with Windows 10 installed). 

Prepare Ubuntu Server (Workstation)
* Check if SSH service is active and start if if needed
  ```bash
  $ service sshd status
  $ service sshd start
  ```
Prepare Windows System (Laptop)
* Install [Putty](https://www.putty.org/)
* Start Putty Desktop App and save a session
  * Enter session name for "Saved Sessions"
  * Enter IP address of workstation
  * "Save" the session
* "Load" the required session and "Open" it
  * Now the login from Windows to the workstation is possible

### File transfer over SSH
When downloading files on the Windows laptop they can be transferred using PSCP from the Windows command shell by typing the following command.
* `<linux user>` &rarr; user name on Linux server
* `<linux pc-name>` &rarr; PC name of the Linux server
* `<source filepath on windows>` &rarr; the source file path on the Windows system
```bash
> pscp <source filepath on windows> <linux user>@<linux pc-name>:/home/<linux user>
```

### Installation of the NVIDIA GPU driver
Before installing the NVIDIA driver the Nouveau driver must be first disabled. 

#### [Disabling Nouveau](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html#runfile-nouveau-ubuntu)
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



Blacklisting nouveau: 

/etc/modprobe.d/blacklist-nvidia-nouveau.conf

https://linuxconfig.org/how-to-disable-nouveau-nvidia-driver-on-ubuntu-18-04-bionic-beaver-linux

In order to download the appropriate NVIDIA GPU driver from the [NVIDIA Driver Downloads](https://www.nvidia.com/Download/index.aspx?lang=en-us) site information about the GPU must be provided.
* Product type (TITAN, GeForce, Quadro, ...)
* Product series
* OS
* etc. ...

I downloaded vesion 418 from to the Windows laptop and transferred the file to the Linux workstation using PSCP as described above.
```bash
> pscp NVIDIA-Linux-x86_64-418.56.run <linux user>@<linux pc-name>:/home/<linux user>
```

The installation was accomplished following the instructions from [NVIDIA Linux-x86_64 418.56](http://us.download.nvidia.com/XFree86/Linux-x86_64/418.56/README/index.html) section 4 [Installing the NVIDIA Driver](http://us.download.nvidia.com/XFree86/Linux-x86_64/418.56/README/installdriver.html).
