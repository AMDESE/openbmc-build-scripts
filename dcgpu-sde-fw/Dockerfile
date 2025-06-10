########################################################
# Copyright (c) 2025 Advanced Micro Devices, Inc. (AMD)
########################################################

FROM ubuntu:22.04

ENV DEBIAN_FRONTEND=noninteractive

# openbmc-build-scripts packages
RUN apt-get update && \
  apt-get install -y \
  chrpath \
  cpio \
  debianutils \
  diffstat \
  file \
  gawk \
  git \
  iputils-ping \
  libdata-dumper-simple-perl \
  liblz4-tool \
  libsdl1.2-dev \
  libthread-queue-any-perl \
  locales \
  python3 \
  socat \
  subversion \
  texinfo \
  vim \
  wget \
  zstd \
  python3-distutils \
  gcc-9 \
  g++-9 \
  software-properties-common \
  && \
  rm -rf /var/lib/apt/lists/*

# Team member requested tools
RUN apt-get update && \
  apt-get install -y \
  curl \
  jq \
  meson \
  ninja-build \
  sudo \
  tree \
  zip

# Downgrade packages for OpenBMC 2.11 products (e.g. MI300X)
# Set GCC 9.5 as the default GCC version
RUN update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-9 60 \
  --slave /usr/bin/g++ g++ /usr/bin/g++-9 && \
  update-alternatives --install /usr/bin/cpp cpp /usr/bin/cpp-9 60

# Add deadsnakes PPA to install Python 3.9
RUN add-apt-repository ppa:deadsnakes/ppa && apt update && apt install -y python3.9 python3.9-venv python3.9-dev

# Set Python 3.9 as the default Python version
RUN update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.9 1

RUN locale-gen en_US.UTF-8
ENV LANG=en_US.UTF-8
ENV LANGUAGE=en_US:en
ENV LC_ALL=en_US.UTF-8

# Enable github runner account
RUN adduser github && \
    usermod -aG sudo github && \
    echo "github ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers.d/github-nopasswd

# Enable service account
ENV SERVICE_ACCOUNT=a1_mi300x_smc
RUN adduser ${SERVICE_ACCOUNT} && \
    echo "${SERVICE_ACCOUNT} ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers.d/${SERVICE_ACCOUNT}-nopasswd && \
    grep -q 1001 /etc/group || groupadd -g 1001 ${SERVICE_ACCOUNT} && \
    grep -q 1001 /etc/passwd || useradd -d /home/${SERVICE_ACCOUNT} -m -u 1001 -g 1001 ${SERVICE_ACCOUNT}

USER ${SERVICE_ACCOUNT}
ENV HOME=/home/${SERVICE_ACCOUNT}
