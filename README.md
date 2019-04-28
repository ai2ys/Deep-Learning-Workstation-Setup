# Deep-Learning-Workstation-Setup
I am sharing my experiences setting up my Deep Learning Workstation. Feel free to contribute.<br>
This work is published under the terms of the [CC-BY-4.0 license](https://creativecommons.org/licenses/by/4.0/legalcode).

## License
<a rel="license" href="http://creativecommons.org/licenses/by/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by/4.0/80x15.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by/4.0/">Creative Commons Attribution 4.0 International License</a>.

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

## OS installation
Although I am used to Windows and newcomer in using Linux, I decided to go with Linux ([Ubuntu Server 18.04 LTS](http://releases.ubuntu.com/18.04/)) because I want to use Docker containers for development. Using Linux and Docker slowed my progress a lot at the beginning, but I believe that I will gain speed later and I this setup will reduce the propability of me damaging the OS. At work I have been using Ubuntu Desktop for a while. As a result of this I chose to go with Ubuntu Server.

### Installing Ubuntu Server 18.04 LTS
Steps acomplished
* Prepare a bootable USB stick with [Ubuntu Server 18.04 LTS](http://releases.ubuntu.com/18.04/)
* Install Ubuntu Server with default values
* Ensure the system is up to date
  ```bash
  sudo apt-get update
  sudo apt-get upgrade
  ```
### Remote access from Windows laptop
I want to remotely access the workstation via SSH from my Windows system (ThinkPad Yoga 380 laptop with Windows 10 installed). 

Prepare Ubuntu Server (Workstation)
* Check if SSH service is active and start if if needed
  ```bash
  service sshd status
  service sshd start
  ```
Prepare Windows System (Laptop)
* Install [Putty](https://www.putty.org/)
* Start Putty Desktop App and save a session
  * Enter session name for "Saved Sessions"
  * Enter IP address of workstation
  * "Save" the session
* "Load" the required session and "Open" it
  * Now the login from Windows to the workstation is possible
 
