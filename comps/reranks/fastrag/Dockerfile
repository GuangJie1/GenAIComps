# Copyright (C) 2024 Intel Corporation
# SPDX-License-Identifier: Apache-2.0

FROM openeuler/openeuler:22.03-lts-sp4

ENV LANG=C.UTF-8
ENV PYTHONPH=/usr/bin/python3

ARG ARCH="cpu"

RUN yum update -y && yum install -y \
    git \
    mesa-libGL \
    jemalloc-devel \
    python3-pip \
    && yum clean all

RUN useradd -m -s /bin/bash user && \
    mkdir -p /home/user && \
    chown -R user /home/user/

USER user

COPY comps /home/user/comps

RUN git clone https://github.com/IntelLabs/fastRAG.git /home/user/fastRAG && \
    cd /home/user/fastRAG && \
    pip3 install --no-cache-dir --upgrade pip && \
    if [ ${ARCH} = "cpu" ]; then \
      pip3 install --no-cache-dir --extra-index-url https://download.pytorch.org/whl/cpu -r /home/user/comps/reranks/fastrag/requirements.txt; \
      pip3 install --no-cache-dir .; \
      pip3 install --no-cache-dir .[intel]; \
    else \
      pip3 install --no-cache-dir -r /home/user/comps/reranks/fastrag/requirements.txt; \
      pip3 install --no-cache-dir .; \
      pip3 install --no-cache-dir .[intel]; \
    fi

ENV PYTHONPATH=$PYTHONPH:/home/user

WORKDIR /home/user/comps/reranks/fastrag

ENTRYPOINT ["python3", "local_reranking.py"]
