# Leveraging GPU support with `docker compose`

Here an example for using `docker compose` with a pre-built image.

More information on how to access GPUs when using `docker compose` can be found on the following page [Enabling GPU access with Compose](https://docs.docker.com/compose/gpu-support/).

## Running the container

The following command will startup all containers specified in the `docker-compose.yml` file. Run the command from the same directory where the file is located.

Options for specifying the GPU index or UUID for the `docker-compose.yml` file. In the `docker-compose.yml` file a variable is used for specifying the GPU index/UUID.

Running `docker compose` will per default use the `docker-compose.yml` file located in the same directory. In the `docker-compose.yml` file a variable has been defined for assigning the GPU to be used. This is very useful when having a system with multiple GPU that might be shared between multiple users.

1. Specify the GPU index/UUID when when running `docker compose up`. The command `GPU_ID=0 docker compose up` will make GPU `0` accessible in the container.

    ```shell
    cd ./examples/nvidia-cuda
    GPU_ID=0 docker compose up
    ```

1. Specify the GPU in the `.env` file and this value will be used when running `docker compose up`.

    ```bash
    # in the './examples/nvidia-cuda/.env' file
    GPU_ID=0
    ```

    The GPU with index `0` will get passed to the container. If the value of `GPU_ID` is not set in the `.env` file, the container will not be able to access the GPU and will run with CPU only.

    ```shell
    docker compose up
    ```

## Removing the container(s)

Run `docker compose down` for stopping and removing the container.

```shell
docker compose down
```
