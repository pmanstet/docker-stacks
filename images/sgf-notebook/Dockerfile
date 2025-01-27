# Copyright (c) Jupyter Development Team.
# Distributed under the terms of the Modified BSD License.

ARG REGISTRY=quay.io
ARG OWNER=jupyter
ARG BASE_IMAGE=$REGISTRY/$OWNER/scipy-notebook
FROM $BASE_IMAGE

LABEL maintainer="Paul Manstetten <paul.manstetten@tuwien.ac.at>"

# iue: keep this (other stacks repeat this too) 
# Fix: https://github.com/hadolint/hadolint/wiki/DL4006
# Fix: https://github.com/koalaman/shellcheck/wiki/SC3014
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root
# iue: install custom kernel dependencies
RUN apt-get update --yes && \
    apt-get install --yes --no-install-recommends \
    # iue: add own dependencies here
    clang \
    clang-tools \ 
    clang-format \    
    clangd \
    cmake && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# iue: keep this (other stacks repeat this too) 
USER ${NB_UID}
# iue: Install Python 3 package dependencies for clang-repl-kernel
RUN mamba install --yes \
    'pip' \
    'hatchling' \
    'ipykernel' \
    'notebook' \
    'nbclassic' \
    'jupyter_server_ydoc' && \
    mamba clean --all -f -y && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"


USER root
# iue: install custom kernel 
COPY sgit.iue/360251/clang_repl_kernel /sgit.iue/360251/clang_repl_kernel
WORKDIR /sgit.iue/360251/clang_repl_kernel
RUN mamba run pip install --user --no-cache-dir --root-user-action=ignore .  && \
    # rm -rf /sgit.iue/360251/clang_repl_kernel && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"
RUN jupyter kernelspec list
RUN update-alternatives --install /usr/bin/clang-repl clang-repl /usr/bin/clang-repl-18 100


USER root
# iue: install sgframework
RUN apt-get update --yes && \
    apt-get install --yes --no-install-recommends \
    # iue: add sgframework dependencies
    gengetopt \
    tree \
    build-essential && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

COPY sgit.iue/360033/sgframework /sgit.iue/360033/sgframework
WORKDIR /sgit.iue/360033/sgframework
ENV DEST_DIR=/usr/local/bin
RUN make config && \
    make && \
    make install
RUN ls -la /usr/local/bin/sg*

USER ${NB_UID}
WORKDIR "${HOME}"
COPY sgit.iue/360033/materials_sources/reference_solutions/3/3min/3Amin/3Amin.sg ./3Amin.sg
COPY sgit.iue/360033/materials_sources/reference_solutions/3/3min/3Amin/mshfile3Amin.sk ./mshfile3Amin.sk
RUN sgrun 3Amin.sg mshfile3Amin
RUN tree -L 3 ./



WORKDIR "${HOME}"
