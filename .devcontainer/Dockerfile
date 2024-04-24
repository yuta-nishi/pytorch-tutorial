FROM nvidia/cuda:11.8.0-cudnn8-devel-ubuntu22.04

# environment
ENV DEBIAN_FRONTEND=noninteractive \
    TZ=Asia/Tokyo \
    CUDA_HOME="/usr/local/cuda" \
    PIP_PREFER_BINARY=1

# workspace
ENV WORK_PATH="/workspace"

WORKDIR ${WORK_PATH}

# apt install
RUN apt-get update -qq && \
    apt-get install -y --no-install-recommends \
    curl \
    git \
    sudo \
    tmux \
    vim \
    wget \
    ssh \
    htop && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* && \
    rm -rf /tmp/* /var/tmp/*

# rye setting
ENV RYE_HOME="/home/duser/.rye" \
    PATH="${RYE_HOME}}/shims:${PATH}"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

RUN curl -sSf https://rye-up.com/get | RYE_INSTALL_OPTION="--yes" bash && \
    source "${RYE_HOME}/env" \
    rye config --set-bool behavior.global-python=true && \
    rye config --set-bool behavior.use-uv=true

# Required for rye
COPY ./.python-version ./pyproject.toml ./requirements* ./README.md ./

RUN source "${RYE_HOME}/env" && \
    rye pin "$(cat .python-version)" && \
    rye sync

# duser setting
ENV USER_ID=3041 \
    GROUP_ID=1000

RUN addgroup --gid ${GROUP_ID} duser && \
    adduser --disabled-password --gecos "" --uid ${USER_ID} --gid ${GROUP_ID} duser && \
    adduser duser sudo && \
    echo "%sudo ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

USER duser

# Need to chown to get permissions
RUN sudo chown -R duser:duser /home/duser
