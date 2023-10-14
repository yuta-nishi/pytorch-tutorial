FROM pytorch/pytorch:2.1.0-cuda12.1-cudnn8-devel

# apt-get
ENV TZ Asia/Tokyo
ENV DEBIAN_FRONTEND noninteractive
RUN apt-get update -qq && \
    apt-get install -y curl git sudo tmux vim wget htop libglib2.0-0 libsm6 libxrender1 libxext6 libgl1-mesa-dev && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* && \
    rm -rf /var/cache/apk/*

# python
ENV WORK_PATH /workspace
WORKDIR $WORK_PATH
ENV PYTHONPATH "/workspace:${PYTHONPATH}"
ENV PIP_OPTIONS "--no-cache-dir --progress-bar off"
COPY ./requirements.txt $WORK_PATH/requirements.txt
RUN pip install ${PIP_OPTIONS} -r $WORK_PATH/requirements.txt && \
    pip install ${PIP_OPTIONS} -U setuptools

# duser setting
ENV USER_ID 10001
ENV GROUP_ID 10001
RUN addgroup --gid $GROUP_ID duser && \
    adduser --disabled-password --gecos "" --uid ${USER_ID} --gid ${GROUP_ID} duser && \
    adduser duser sudo && \
    echo "%sudo ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

USER duser
