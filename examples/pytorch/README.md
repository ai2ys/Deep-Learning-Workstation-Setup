# PyTorch example

Deriving Docker images from the `nvcr.io/nvidia/pytorch:<xx.xx>-py3` Docker images.

For details about the specific images defined by the tags checkout [https://docs.nvidia.com/deeplearning/frameworks/pytorch-release-notes/index.html](https://docs.nvidia.com/deeplearning/frameworks/pytorch-release-notes/index.html).

Run `nvidia-smi` on your system and check the output. Here an example from my system.

```shell
$ nvidia-smi
Sat Mar  4 22:03:47 2023
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 525.85.12    Driver Version: 525.85.12    CUDA Version: 12.0     |
|-------------------------------+----------------------+----------------------+
```

Select a Docker image tag from the repository with a CUDA version less or equal to the CUDA version listed from the `nvidia-smi` output.
On my system I select an image tag `22.10-py3` using CUDA `11.8.0`, see [https://docs.nvidia.com/deeplearning/frameworks/pytorch-release-notes/rel-22-10.html#rel-22-10](https://docs.nvidia.com/deeplearning/frameworks/pytorch-release-notes/rel-22-10.html#rel-22-10).

## Building the image

Run the following command to build the image.

```shell
cd ./examples/pytorch
docker compose build
```

## Start the container and run the Jupyter notebook

After the image has been built successfully, start the container. The command below will make the GPU with index `0` accessible in the container.

```shell
cd ./examples/pytorch
GPU_ID=0 docker compose up
```

Open JupyterLab from the link in the terminal output and run the cells of the Jupyter notebook provided `check-gpu-support.ipynb`.

To stop the container you can press `CMD/CTRL+C`.

## Removing the container(s)

Run `docker compose down` for stopping and removing the container.

```shell
cd ./examples/pytorch
docker compose down
```
