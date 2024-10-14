# Copyright (C) 2024 Intel Corporation
# SPDX-License-Identifier: Apache-2.0

FROM openeuler/openeuler:22.03-lts-sp4

ENV PYTHONPATH=/usr/bin/python3

ARG ARCH="cpu"

RUN yum update -y && \
    yum install -y --setopt=install_weak_deps=False \
    mesa-libGL \
    python3-pip \
    jemalloc-devel && \
    yum clean all

RUN useradd -m -s /bin/bash user && \
    mkdir -p /home/user && \
    chown -R user:user /home/user/

COPY comps /home/user/comps

USER user

RUN pip3 install --no-cache-dir --upgrade pip && \
    if [ "${ARCH}" = "cpu" ]; then pip3 install --no-cache-dir torch torchvision --index-url https://download.pytorch.org/whl/cpu; fi && \
    pip3 install --no-cache-dir -r /home/user/comps/retrievers/redis/langchain/requirements.txt

ENV PYTHONPATH=$PYTHONPATH:/home/user

WORKDIR /home/user/comps/retrievers/redis/langchain

ENTRYPOINT ["python", "retriever_redis.py"]
