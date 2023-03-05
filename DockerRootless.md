# Instructions

Following instruction from [https://docs.docker.com/engine/security/rootless/](https://docs.docker.com/engine/security/rootless/).

---

## Stop system-wide Docker daemon

1. Disable the system-wide service

    ```bash
    sudo systemctl disable --now docker.service docker.socket
    ```

1. Check the status of the service

    ```bash
    sudo systemctl status docker
    ```

    This should look like this:

    ```shell
    $ sudo systemctl status docker
    ● docker.service - Docker Application Container Engine
         Loaded: loaded (/lib/systemd/system/docker.service; disabled; vendor preset: enabled)
         Active: inactive (dead)
    TriggeredBy: ● docker.socket
           Docs: https://docs.docker.com
    ```

1. Reboot the system

    ```bash
    sudo reboot
    ```

## Install prerequisites

The following packages are required for rootless Docker:

- `uidmap`
- `dbus-user-session`

Check if they are already installed on your system.

```shell
$ dpkg -l | grep 'uidmap\|dbus-user-session'
ii  dbus-user-session                          1.12.16-2ubuntu2.3                  amd64        simple interprocess messaging system (systemd --user integration)
ii  uidmap                                     1:4.8.1-1ubuntu5.20.04.4            amd64        programs to help use subuids
```

Install the previous mentioned packages if they are not already installed.

```bash
sudo apt-get update
sudo apt-get install -y uidmap dbus-user-session 
```

Required to login as a specific user to install and start rootless Docker.

```bash
sudo apt-get update
sudo apt-get install -y systemd-container
```

## Enabling GPU support

GPU support in Docker rootless mode requires some changes to the following configuration file: `/etc/nvidia-container-runtime/config.toml`

```yml
[nvidia-container-cli]
# ...
no-cgroups = true
# ...

[nvidia-container-runtime]
# ...
# debug = "~/.local/nvidia-container-runtime.log"
# ...
```

## Install rootless Docker

The following instructions assume that Docker has been installed previously as system-wide service and the script `dockerd-rootless-setuptool.sh` is available on the system.

When using Docker rootless Docker has to get installed for each user on the system separately. Follow the instructions below for each user that needs Docker.

1. Define variable with the user name you like to install Docker (rootless mode)

    ```bash
    other_user=<user name>
    ```

1. Login as user using `machinectl` and open `bash`

    ```bash
    sudo machinectl shell $other_user@
    bash
    ```

    1. Install rootless docker
    
        ```bash
        dockerd-rootless-setuptool.sh install
        ```

    1. Append variables mentioned at the bottom of the output to `~/.bashrc` file
    
        ```bash
        echo """
        export PATH=/usr/bin:\$PATH
        export DOCKER_HOST=unix:///run/user/$(id -u $USER)/docker.sock
        """ >> ~/.bashrc
        ```

    1. Check if the appending the variables was successful using
    
        ```bash
        tail --lines 3 ~/.bashrc
        ```
    
        Should look similar to this:
    
        ```shell
        $ tail --lines 3 ~/.bashrc
        export PATH=/usr/bin:$PATH
        export DOCKER_HOST=unix:///run/user/<user id>/docker.sock
        ```

    1. Making the previous added variable available from command line
    
        ```bash
        source ~/.bashrc
        ```

    1. Data-root directory can be changed before start if you want to store the Docker images on a different location.
    
        1. Specify the variable `data_root` and create the directory if it does not already exist
    
            ```bash
            data_root=<specify alternate data-root directory>
            mkdir ${data_root}
            ```
    
        1. Create file `~/.config/docker/daemon.json` with alternate data-root location
    
            ```bash
            echo """{
                \"data-root\":\"${data_root}\"
            }""" > ~/.config/docker/daemon.json
            ```
    
        1. Check if the alternate data-root has been added to the file
    
            ```bash
            cat ~/.config/docker/daemon.json
            ```

    1. Start Docker (rootless mode) for user
    
        ```shell
        systemctl --user start docker
        systemctl --user enable docker
        systemctl --user status docker
        ```

        The status should look similar to this, where the circle on the left should be green indicating the running service (`🟢 docker.service - ...`.
    
        ```shell
        $ systemctl --user status docker
        ● docker.service - Docker Application Container Engine (Rootless)
             Loaded: loaded (/home/sylvia/.config/systemd/user/docker.service; enabled; vendor preset: enabled)
             Active: active (running) since Sun 2023-03-05 15:50:01 CET; 30min ago
               Docs: https://docs.docker.com/go/rootless/
        ...
        ```

    1. Check if alternate data-root gets applied, if defined previously
    
        ```shell
        docker info
        ```

    1. Exit bash and user logon using `machinectl`
    
        ```bash
        exit
        exit
        ```

1. Enable Docker service startup on system startup for user

    ```bash
    sudo loginctl enable-linger $other_user
    ```
