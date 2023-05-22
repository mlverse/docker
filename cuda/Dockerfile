ARG DISTRIBUTION=focal
ARG R_VERSION=release
ARG CUDA_VERSION=11.6.2
ARG CUDNN_VERSION=8

FROM nvidia/cuda:${CUDA_VERSION}-cudnn${CUDNN_VERSION}-devel-ubuntu20.04 as focal
COPY focal-Rprofile.site /tmp/Rprofile.site
FROM nvidia/cuda:${CUDA_VERSION}-cudnn${CUDNN_VERSION}-devel-ubuntu18.04 as bionic
COPY bionic-Rprofile.site /tmp/Rprofile.site

FROM ${DISTRIBUTION}

RUN apt-get update -qq && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y \
    apt-transport-https \
    curl \
    fontconfig \
    libcurl4-openssl-dev \
    locales \
    perl \
    sudo \
    tzdata \
    git \
    wget && \
    rm -rf /var/lib/apt/lists/*

# Install TinyTeX
RUN wget -qO- "https://yihui.org/tinytex/install-bin-unix.sh" | sh && \
    /root/.TinyTeX/bin/*/tlmgr path remove && \
    mv /root/.TinyTeX/ /opt/TinyTeX && \
    /opt/TinyTeX/bin/*/tlmgr option sys_bin /usr/local/bin && \
    /opt/TinyTeX/bin/*/tlmgr path add

# Install pandoc
RUN mkdir -p /opt/pandoc && \
    wget -O /opt/pandoc/pandoc.gz https://files.r-hub.io/pandoc/linux-64/pandoc.gz && \
    gzip -d /opt/pandoc/pandoc.gz && \
    chmod +x /opt/pandoc/pandoc && \
    ln -s /opt/pandoc/pandoc /usr/bin/pandoc && \
    wget -O /opt/pandoc/pandoc-citeproc.gz https://files.r-hub.io/pandoc/linux-64/pandoc-citeproc.gz && \
    gzip -d /opt/pandoc/pandoc-citeproc.gz && \
    chmod +x /opt/pandoc/pandoc-citeproc && \
    ln -s /opt/pandoc/pandoc-citeproc /usr/bin/pandoc-citeproc

# Set default locale
ENV LANG C.UTF-8

# Set default timezone
ENV TZ UTC

RUN curl -Ls https://github.com/r-lib/rig/releases/download/latest/rig-linux-latest.tar.gz | sudo tar xz -C /usr/local && \
  rig add ${R_VERSION} && \
  rig default "$(rig list)" && \
  rm -rf /var/lib/apt/lists/*

RUN Rscript -e "file.copy('tmp/Rprofile.site', file.path(R.home(), 'etc', 'Rprofile.site'))"
