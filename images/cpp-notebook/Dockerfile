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
    lldb \ 
    gdb \
    procps \
    vim \
    htop \
    cmake && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

RUN update-alternatives --install /usr/bin/clang-repl clang-repl /usr/bin/clang-repl-18 100

RUN mamba install --yes 'pip' && \
    mamba clean --all -f -y

RUN mamba run pip install --root-user-action=ignore 'clang-repl>=1.0.4'

ENV CODE_VERSION=4.97.2
RUN curl -fOL https://github.com/coder/code-server/releases/download/v$CODE_VERSION/code-server_${CODE_VERSION}_amd64.deb \
    && dpkg -i code-server_${CODE_VERSION}_amd64.deb \
    && rm -f code-server_${CODE_VERSION}_amd64.deb

RUN mamba run pip install --root-user-action=ignore 'jupyter-server-proxy'
RUN mamba run pip install --root-user-action=ignore 'jupyter-vscode-proxy'

# note codelldb (ms C/C++ is not available): not universal, need to select corrent arch (here linux-x64, s.th. no bootstrap is required later)
RUN curl -fOL https://github.com/vadimcn/codelldb/releases/download/v1.11.4/codelldb-linux-x64.vsix
RUN curl -fOL https://open-vsx.org/api/ms-vscode/cmake-tools/1.20.52/file/ms-vscode.cmake-tools-1.20.52.vsix
RUN curl -fOL https://open-vsx.org/api/llvm-vs-code-extensions/vscode-clangd/0.1.33/file/llvm-vs-code-extensions.vscode-clangd-0.1.33.vsix
RUn curl -fOL https://open-vsx.org/api/ms-python/python/2025.0.0/file/ms-python.python-2025.0.0.vsix
RUn curl -fOL https://open-vsx.org/api/ms-toolsai/jupyter/2025.1.0/file/ms-toolsai.jupyter-2025.1.0.vsix

RUN code-server \
    --extensions-dir "/usr/lib/code-server/lib/vscode/extensions/" \
    --install-extension ms-toolsai.jupyter-2025.1.0.vsix \
    --install-extension ms-python.python-2025.0.0.vsix \
    --install-extension llvm-vs-code-extensions.vscode-clangd-0.1.33.vsix \
    --install-extension ms-vscode.cmake-tools-1.20.52.vsix \
    --install-extension codelldb-linux-x64.vsix

RUN rm -rf *.vsix

RUN code-server --version
RUN code-server --extensions-dir "/usr/lib/code-server/lib/vscode/extensions/" --list-extensions
RUN clang-format --version
RUN clangd --version
RUN gcc --version
RUN gdb --version
RUN lldb --version
RUN clang --version
RUN clang-repl --version
RUN jupyter kernelspec list

USER ${NB_UID}
WORKDIR "${HOME}"

