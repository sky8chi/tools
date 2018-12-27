FROM centos:7
MAINTAINER tianxiang.chi <sky8chi@163.com>

RUN yum install -y gdb gdb-gdbserver openssh-server sudo

RUN echo -e '#!/bin/bash\nadduser deploy; \n echo deploy |passwd --stdin deploy' >> create_deploy.sh && \
    chmod 755 create_deploy.sh && \
    ./create_deploy.sh && \
    gpasswd -a deploy root && \
    rm create_deploy.sh && \
    chmod 644 /etc/sudoers && \
    echo "deploy ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

RUN yum install -y wget gcc gcc-c++ make
ARG CMAKE_VERSION=3.13.1
RUN mkdir -p /data/install && cd /data/install && wget https://cmake.org/files/v${CMAKE_VERSION%.*}/cmake-${CMAKE_VERSION}.tar.gz \
&& tar -zxvf cmake-${CMAKE_VERSION}.tar.gz && rm cmake-${CMAKE_VERSION}.tar.gz && cd cmake-${CMAKE_VERSION} \
&& ./bootstrap && make && make install 

RUN yum install -y texinfo
ARG GDB_VERSION=8.1
RUN cd /data/install && wget https://mirrors.ustc.edu.cn/gnu/gdb/gdb-${GDB_VERSION}.tar.gz \
&& tar -zxvf gdb-${GDB_VERSION}.tar.gz && rm gdb-${GDB_VERSION}.tar.gz && cd gdb-${GDB_VERSION} \
&& ./configure && make && make install 
RUN ssh-keygen -t rsa -f /etc/ssh/ssh_host_ed25519_key
RUN ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key
RUN ssh-keygen -t rsa -f /etc/ssh/ssh_host_ecdsa_key
USER deploy

WORKDIR /data/projects
ENV LC_ALL en_US.UTF-8
ENV TZ Asia/Shanghai
