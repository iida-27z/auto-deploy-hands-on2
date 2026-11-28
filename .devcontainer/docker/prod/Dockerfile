FROM eclipse-temurin:27.0.1.1_4-jre@sha256:9723bc085e2a7bd142b78c9a293427941c309962276d8ef28053a61f73238930

WORKDIR /app

RUN apt update && apt install -y openssh-server && \
    apt clean && \
    rm -rf /var/lib/apt/lists/*

ARG GROUP_NAME=deployer
ARG USER_NAME=deployer
RUN groupadd -r ${GROUP_NAME} && useradd -r -g ${GROUP_NAME} -m -s /bin/bash ${USER_NAME} \
    && mkdir -p /home/${USER_NAME}/.ssh /var/run/sshd \
    && chown -R ${USER_NAME}:${GROUP_NAME} /home/${USER_NAME}

RUN chown -R ${USER_NAME}:${GROUP_NAME} /app
