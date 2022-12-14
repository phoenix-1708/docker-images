# Copyright (c) 2016-2022 Crave.io Inc. All rights reserved
FROM fedora:latest
LABEL org.opencontainers.image.author.name="Crave.io Inc." \
      org.opencontainers.image.author.email="contact@crave.io"

# Install the minimum set of tools
RUN dnf update -y \
 && dnf -y install \
    bash-completion \
    file \
    git \
    gcc-c++ \
    jq \
    make \
    openssh-server \
    redhat-lsb-core \
    subversion \
    sudo \
    vim-enhanced \
# Install compilation tools
 && dnf -y install @development-tools \
 && dnf -y group install "C Development Tools and Libraries" \
 && echo ". /etc/bash_completion" >> /root/.bashrc \
# Non root user
 && useradd -ms /bin/bash admin \
 && echo "admin:admin" | chpasswd \
 && usermod -a -G wheel admin \
# Add to sudoers
 && echo "admin ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers \
 && mkdir -p /var/run/sshd

COPY sshd_config /etc/ssh/sshd_config

USER admin
ENV HOME=/home/admin USER=admin
WORKDIR /home/admin
CMD /bin/bash

RUN sudo chown -R admin:admin /home/admin \
 && echo "alias ls='ls --color' ; alias ll='ls -l'" >> /home/admin/.bashrc \
 && mkdir /home/admin/.ssh \
 && chmod 700 /home/admin/.ssh \
 && touch /home/admin/.ssh/authorized_keys \
 && sudo chmod 0600 /etc/ssh/*

EXPOSE 22
