#
# Copyright (c) 2019 - UPMEM
#

## Bare environment created to build the UPMEM SDK

FROM ubuntu:18.04
LABEL maintainer="sw@upmem.com"

# -------------------------------------------------------------------
# Embedded packages: the bare minimum to build the entire toolchain
# default-jdk is required with Ubuntu to avoid an error about JNI in the backends module

ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y \
    clang-format-6.0 \
    default-jdk \
    doxygen \
    expect \
    gcc-8 \
    g++-8 \
    git \
    graphviz \
    libedit-dev \
    libelf-dev \
    libncurses5-dev \
    libnuma-dev \
    libudev-dev \
    libxml2-dev \
    make \
    maven \
    ninja-build \
    openjdk-11-jdk \
    pkg-config \
    python \
    python-pip \
    libpython-dev \
    libpython3-dev \
    python3-pip \
    python3-serial \
    python3 \
    lsb-release \
    sudo \
    swig \
    vim \
    valgrind \
    wget \
    libusb-1.0-0-dev \
    libftdi-dev

# default cmake version is not recent enough
RUN pip3 install 'cmake==3.13.0' sphinx sphinx_rtd_theme sphinx-tabs autopep8 ctypesgen pylint pytest pytest-cov pyyaml 'pyelftools>=0.26' tabulate

# Use GCC-8 as default compiler
RUN update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-8 80 \
    --slave /usr/bin/g++ g++ /usr/bin/g++-8
RUN update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-7 70 \
    --slave /usr/bin/g++ g++ /usr/bin/g++-7

# -------------------------------------------------------------------
# Specific part for Jenkins agent: ssh daemon

RUN apt-get install -y openssh-server
RUN mkdir /var/run/sshd
CMD /usr/sbin/sshd && bash

# -------------------------------------------------------------------
# Create upmem user

RUN adduser --disabled-password upmem
RUN adduser upmem sudo
RUN echo 'upmem:upmem' | chpasswd
RUN echo 'upmem ALL=(ALL) NOPASSWD: ALL' >> /etc/sudoers

#RUN chown -R upmem:upmem /home/upmem

# -------------------------------------------------------------------
# Set system gitconfig with caching of credential
RUN git config --system credential.helper cache

EXPOSE 22
