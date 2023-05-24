ARG DISTRIBUTION="focal"
ARG R_VERSION="release"

FROM ubuntu:focal as focal
COPY focal-Rprofile.site /tmp/Rprofile.site
FROM ubuntu:bionic as bionic
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
  rig add ${R_VERSION} --without-pak && \
  rm -rf /var/lib/apt/lists/* && \
  rm -rf /tmp/rig && \
  apt-get clean

# We want to install pak into the system library, so we don't do it with rig
RUN for inst in $(ls /opt/R/*/lib/R/bin/INSTALL); do \
      sed -i '1s/^/#!\/bin\/bash\n/' ${inst}; \
    done; \
    versions="$(rig ls --json | grep '"name"' | sed 's/^.*\"name\": \"//' | sed 's/\",//')" && \
    for ver in $versions; do \
      R-${ver} -q -e \
      'install.packages("pak", lib = .Library, repos = sprintf("https://r-lib.github.io/p/pak/%s/%s/%s/%s", "devel", .Platform$pkgType, R.Version()$os, R.Version()$arch))'; \
    done

# Install openBLAS which is critical for torch performance
RUN apt-get update -qq && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y \
    libopenblas-base && \
    rm -rf /var/lib/apt/lists/* 
