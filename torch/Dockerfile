ARG RUNTIME="cpu"
ARG DISTRIBUTION="focal"
ARG R_VERSION="release"
ARG CUDA_VERSION="11.7.1"

FROM ghcr.io/mlverse/cpu:${R_VERSION}-${DISTRIBUTION} as cpu
FROM ghcr.io/mlverse/cuda:${CUDA_VERSION}-${R_VERSION}-${DISTRIBUTION} as cuda

FROM ${RUNTIME}

ARG TORCH_VERSION="v0.10.0"

RUN export V="${TORCH_VERSION}" && \
    export TORCH_INSTALL=1 && \
    Rscript -e "v <- Sys.getenv('V'); if (v == 'cran') pak::pkg_install('torch') else pak::pkg_install(sprintf('mlverse/torch@%s', v));" && \
    Rscript -e "pak::pak_cleanup(force=TRUE)"