# Copyright (c) Jupyter Development Team.
# Distributed under the terms of the Modified BSD License.

ARG REGISTRY=quay.io
ARG OWNER=jupyter
ARG BASE_IMAGE=$REGISTRY/$OWNER/scipy-notebook
FROM $BASE_IMAGE

LABEL maintainer="Paul Manstetten <paul.manstetten@tuwien.ac.at>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root
# iue: install dependencies
RUN apt-get update --yes && \
    apt-get install --yes --no-install-recommends \
    clang \
    clang-tools \ 
    clang-format \
    clangd \
    cmake && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

RUN update-alternatives --install /usr/bin/clang-repl clang-repl /usr/bin/clang-repl-18 100

USER ${NB_UID}

# iue: install custom kernel
RUN mamba install --yes \
    'pip' && \
    mamba clean --all -f -y && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"

RUN mamba run pip install --user clang-repl  && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"

RUN clang-repl --version
RUN jupyter kernelspec list

WORKDIR "${HOME}"
