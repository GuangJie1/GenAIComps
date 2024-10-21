# Copyright (C) 2024 Intel Corporation
# SPDX-License-Identifier: Apache-2.0

FROM openeuler/openeuler:24.03-lts

ENV LANG=C.UTF-8
ENV PYTHONPATH=/usr/bin/python

ARG ARCH="cpu"

RUN yum -y update && \
    yum install -y --setopt=install_weak_deps=False \
    mesa-libGL \
    python-pip \
    shadow-utils \
    jemalloc-devel && \
    yum clean all

RUN useradd -m -s /bin/bash user && \
    mkdir -p /home/user && \
    chown -R user /home/user/

USER user

COPY comps /home/user/comps

RUN pip install --no-cache-dir --upgrade pip && \
    if [ ${ARCH} = "cpu" ]; then pip install --no-cache-dir torch --index-url https://download.pytorch.org/whl/cpu; fi && \
    pip install --no-cache-dir -r /home/user/comps/reranks/tei/requirements.txt

ENV PYTHONPATH=$PYTHONPATH:/home/user

WORKDIR /home/user/comps/reranks/tei

ENTRYPOINT ["python", "reranking_tei.py"]
