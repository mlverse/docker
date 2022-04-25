# Docker images with GPU support

<!-- badges: start -->

<!-- badges: end -->

## cuda

Images in [mlverse/cuda](https://github.com/mlverse/docker/pkgs/container/cuda) can be used with for example:

    docker run -it --gpus all ghcr.io/mlverse/cuda:11.6.2-4.2-focal R

It's recommended to always use a fixed tag like in the above example: `11.6.2-4.2-focal`. Tags are created with the pattern: `CUDA_VERSION-R_VERSION-DISTRIBUTION`.

| Pattern        | Example | Description                                                                                  |
|----------------|---------|----------------------------------------------------------------------------------------------|
| `CUDA_VERSION` | 11.6.2  | CUDA version including patch version.                                                        |
| `R_VERSION`    | 4.1     | R version without the patch version. We also regularly build `release` and `devel` versions. |
| `DISTRIBUTION` | focal   | The Ubuntu distribution. Currently `focal` and `bionic` are supported.                       |
