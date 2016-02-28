FROM ubuntu:xenial

RUN sed -i.bak -e "s%archive.ubuntu.com/ubuntu/%ftp.riken.jp/Linux/ubuntu/%g" /etc/apt/sources.list
RUN apt-get update
RUN apt-get -y install aptitude
RUN aptitude update
RUN aptitude -y install git \
    python \
    openssh-server \
    sudo \
    supervisor \
    build-essential \
    vim \
    iproute2 \
    systemd

EXPOSE 22

RUN useradd -m -d /home/docker -u 1000 -s /bin/bash docker
RUN mkdir /home/docker/.ssh
RUN chmod 700 /home/docker/.ssh
ADD authorized_keys /home/docker/.ssh/
RUN chmod 600 /home/docker/.ssh/authorized_keys
RUN chown -R docker:docker /home/docker
RUN mv /home/docker/.ssh /home/docker/ssh && mv /home/docker/ssh /home/docker/.ssh
RUN mkdir /var/run/sshd
RUN echo 'docker ALL=(ALL:ALL) NOPASSWD:ALL' >> /etc/sudoers
RUN echo 'docker:docker' | chpasswd
RUN systemctl enable ssh
RUN systemctl enable supervisor
RUN locale-gen en_US.UTF-8
RUN locale-gen ja_JP.UTF-8

ENTRYPOINT ["/sbin/init", "3"]
