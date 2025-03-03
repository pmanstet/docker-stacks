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
    gengetopt && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

RUN mamba install --yes 'pip' && \
    mamba clean --all -f -y

RUN git clone https://gitlab.tuwien.ac.at/e360/sgframework.git && cd sgframework && \
    export DEST_DIR=/usr/local/bin && \
    make config && \
    make && \
    make install

ENV CODE_VERSION=4.97.2
RUN curl -fOL https://github.com/coder/code-server/releases/download/v$CODE_VERSION/code-server_${CODE_VERSION}_amd64.deb \
    && dpkg -i code-server_${CODE_VERSION}_amd64.deb \
    && rm -f code-server_${CODE_VERSION}_amd64.deb

RUN mamba run pip install --root-user-action=ignore 'jupyter-server-proxy'
RUN mamba run pip install --root-user-action=ignore 'jupyter-vscode-proxy'

RUN curl -fOL https://open-vsx.org/api/ms-python/python/2025.0.0/file/ms-python.python-2025.0.0.vsix

RUN code-server \
    --extensions-dir "/usr/lib/code-server/lib/vscode/extensions/" \
    --install-extension ms-python.python-2025.0.0.vsix \
    --install-extension sgframework/sgframework-highlighter-0.0.1.vsix 

RUN rm -rf sgframework

RUN code-server --version
RUN code-server --extensions-dir "/usr/lib/code-server/lib/vscode/extensions/" --list-extensions
RUN gcc --version

USER ${NB_UID}
WORKDIR "${HOME}"

