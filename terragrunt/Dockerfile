# Container to run Terragrunt and Terraform
# Contains inside mise to allow users to install custom Terraform and Terragrunt versions
FROM ubuntu:22.04
LABEL maintainer="Gruntwork <info@gruntwork.io>"

ARG MISE_VERSION_INSTALL=v2024.4.0

ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update \
    && apt-get install -y \
    curl \
    ca-certificates \
    make \
    git \
    jq \
    unzip \
    wget \
    sudo \
    && rm -rf /var/lib/apt/lists/* \
    && sudo install -m 0755 -d /etc/apt/keyrings \
    && sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc \
    && sudo chmod a+r /etc/apt/keyrings/docker.asc \
    && echo \
      "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
      $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
      sudo tee /etc/apt/sources.list.d/docker.list > /dev/null \
    && sudo apt-get update \
    && sudo apt-get install -y docker-ce docker-ce-cli containerd.io
RUN mkdir -p /root/.ssh
COPY ./known_hosts /root/.ssh/known_hosts
RUN chown -R root:root /root/.ssh
# install mise
RUN curl https://mise.run | MISE_VERSION=${MISE_VERSION_INSTALL} MISE_INSTALL_PATH=/usr/bin/mise sh

ENV PATH="~/.local/share/mise/shims:${PATH}"

ENV TF_INPUT=false
ENV TF_IN_AUTOMATION=1
