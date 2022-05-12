# [Choice] Python version (use -bullseye variants on local arm64/Apple Silicon): 3, 3.10, 3.9, 3.8, 3.7, 3.6, 3-bullseye, 3.10-bullseye, 3.9-bullseye, 3.8-bullseye, 3.7-bullseye, 3.6-bullseye, 3-buster, 3.10-buster, 3.9-buster, 3.8-buster, 3.7-buster, 3.6-buster
ARG VARIANT=3
FROM --platform=linux/amd64 mcr.microsoft.com/vscode/devcontainers/python:0-${VARIANT}

ENV PYTHONUNBUFFERED 1

# [Choice] Node.js version: none, lts/*, 16, 14, 12, 10
ARG NODE_VERSION="none"
RUN if [ "${NODE_VERSION}" != "none" ]; then su vscode -c "umask 0002 && . /usr/local/share/nvm/nvm.sh && nvm install ${NODE_VERSION} 2>&1"; fi


# https://github.com/debuerreotype/debuerreotype/issues/10
RUN seq 1 8 | xargs -I{} mkdir -p /usr/share/man/man{}

# Install deb packages
RUN apt-get update && apt-get install -y --no-install-recommends \
    poppler-utils \
    postgresql-client \
    libmagickwand-dev \
    libpq-dev \
    build-essential \
 && rm /etc/ImageMagick-6/policy.xml \
 && rm -rf /var/lib/{apt,dpkg,cache,log}/

# Set --build-arg FONDUER_VERSION=X.X.X to install a specific version of
# Fonduer, otherwise the lastest version is installed.
ARG FONDUER_VERSION=

# Install python packages
RUN pip install --upgrade pip
RUN pip install --no-cache-dir \
    torch==1.6.0+cpu -f https://download.pytorch.org/whl/torch_stable.html \
    fonduer${FONDUER_VERSION:+==${FONDUER_VERSION}} \
    matplotlib \
    ipywidgets \
    jupyter \
    ipykernel \
    && python -m spacy download en
