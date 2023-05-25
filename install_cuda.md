# Install CUDA

With this steps you'll be able to install `CUDA` using `conda`.

> It is recommended to install `CUDA` in the "base" conda env.

```console
# install nvidia-cuda-toolkit
$ conda install -c "nvidia/label/cuda-11.8.0" cuda-toolkit

# install nvidia-drivers
$ sudo apt install nvidia-utils-530
```