FROM ubuntu:26.10@sha256:29ac5e68db830ab61b7d77c1ffdb78c73f0631de88a5d77bf4399868f64f38dd

WORKDIR /actions-runner

ARG ACTIONS_RUNNER_VERSION=2.338.2
RUN apt update && apt install -y curl tar jq build-essential libicu-dev libssl-dev openssh-client openssh-server && \
    cd /actions-runner && \
    curl -o actions-runner-linux-x64-${ACTIONS_RUNNER_VERSION}.tar.gz -L https://github.com/actions/runner/releases/download/v${ACTIONS_RUNNER_VERSION}/actions-runner-linux-x64-${ACTIONS_RUNNER_VERSION}.tar.gz && \
    tar xzf ./actions-runner-linux-x64-${ACTIONS_RUNNER_VERSION}.tar.gz && \
    ./bin/installdependencies.sh && \
    apt clean && \
    rm -rf /var/lib/apt/lists/*

ARG GROUP_NAME=runner
ARG USER_NAME=runner
RUN groupadd -r ${GROUP_NAME} && useradd -r -g ${GROUP_NAME} -m -s /bin/bash ${USER_NAME} \
    && mkdir -p /home/${USER_NAME}/.ssh /var/run/sshd \
    && chown -R ${USER_NAME}:${GROUP_NAME} /home/${USER_NAME}

USER root

RUN chown -R ${USER_NAME}:${GROUP_NAME} /actions-runner
