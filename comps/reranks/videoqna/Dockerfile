
# Copyright (C) 2024 Intel Corporation
# SPDX-License-Identifier: Apache-2.0

FROM openeuler/openeuler:22.03-lts-sp4

ENV LANG=C.UTF-8
ENV PYTHONPATH=/usr/bin/python3


RUN yum update -y && yum -y install python3-pip

RUN useradd -m -s /bin/bash user && \
    mkdir -p /home/user && \
    chown -R user /home/user/

USER user

COPY comps /home/user/comps

RUN pip3 install --no-cache-dir --upgrade pip && \
pip3 install --no-cache-dir -r /home/user/comps/reranks/videoqna/requirements.txt

ENV PYTHONPATH=$PYTHONPATH:/home/user

WORKDIR /home/user/comps/reranks/videoqna

ENTRYPOINT ["python", "local_reranking.py"]