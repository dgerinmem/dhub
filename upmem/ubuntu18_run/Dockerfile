#
# Copyright (c) 2019 - UPMEM
#

## Bare environment created to use the UPMEM SDK

FROM ubuntu:18.04
LABEL maintainer="sw@upmem.com"

# -------------------------------------------------------------------
# Embedded packages

RUN apt-get update && apt-get install -y \
    cmake \
    gcc-8 \
    g++-8 \
    git \
    gnupg \
    libnuma1 \
    libpython2.7 \
    make \
    openjdk-11-jdk \
    pkg-config \
    sudo \
    vim \
    wget

RUN pip3 install pyyaml 'pyelftools>=0.26'

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
EXPOSE 22

# -------------------------------------------------------------------
# Create upmem user

RUN adduser --disabled-password upmem
RUN adduser upmem sudo
RUN echo 'upmem:upmem' | chpasswd
RUN echo 'upmem ALL=(ALL) NOPASSWD: ALL' >> /etc/sudoers

#RUN chown -R upmem:upmem /home/upmem

#  --------------------------------------------------------------------
#  Install upmem SDK not using docker cache to make sure wget is performed
ARG CACHEBUST=1

COPY ./upmem_2021.3.0_amd64.deb  /home/upmem/upmem_sdk_amd64.deb
RUN apt-get install -y /home/upmem/upmem_sdk_amd64.deb && \
    rm -f /home/upmem/upmem_sdk_amd64.deb


# -------------------------------------------------------------------
# HEDGES specific python2.7 + numpy env install

# basic python on system
RUN apt -y install python lsb-release unzip
RUN apt -y install python-pip
RUN apt -y install python-dev
RUN apt -y install python-dev
RUN apt -y install python-setuptools
RUN apt -y install virtualenv

# required python2.7 sytem alternative install 
RUN apt -y install libreadline-gplv2-dev libncursesw5-dev libssl-dev libsqlite3-dev tk-dev libgdbm-dev libc6-dev libbz2-dev
RUN apt -y install libffi-dev
RUN apt -y install liblzma-dev lzma
RUN wget https://www.python.org/ftp/python/2.7/Python-2.7.tgz && tar -xzf Python-2.7.tgz && cd Python-2.7 && ./configure && make &&  make altinstall
RUN python2 -m pip install --user Cython

# numpy dev mode
RUN git clone https://github.com/numpy/numpy && cd numpy && git checkout v1.13.3 && git submodule update --init --recursive  && python setup.py install
