ARG BASE_IMAGE=ubuntu:20.04
ARG PX4_Version=v1.13.2

FROM ${BASE_IMAGE}

ENV TZ=America/Indiana/Indianapolis
ENV UserName=codexlabs
USER root

RUN echo 'debconf debconf/frontend select Noninteractive' | debconf-set-selections && \
    ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt-get update && \
    apt-get install -y \
    cmake \
    make \
    vim \
    screen \
    sudo

RUN adduser --force-badname --disabled-password --gecos '' --shell /bin/bash ${UserName} && \
	echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers && \
	usermod -a -G sudo ${UserName} && \
	usermod -a -G audio ${UserName} && \
	usermod -a -G video ${UserName}

USER ${UserName}
WORKDIR /home/${UserName}

RUN sudo apt-get update && \
    sudo apt-get install -y \
    python3-dev \
    python3-numpy \
    gcc \
    g++ \
    unzip \
    curl \
    wget \
    git \
    lsb-core

COPY . /home/${UserName}/PX4-Autopilot
# Install PX4
RUN sudo chown -R ${UserName} PX4-Autopilot && \
    cd PX4-Autopilot && \
    bash ./Tools/setup/ubuntu.sh --no-sim-tools && \
    make

CMD ["bash"]
