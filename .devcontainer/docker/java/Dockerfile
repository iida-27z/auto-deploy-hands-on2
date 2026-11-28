FROM node:26.13.1-trixie-slim@sha256:abe7e5a82b61a8ef67b315b85497c9e65173387ccf2f4a029248184b8e020be5 AS node-env
FROM eclipse-temurin:25.0.5.1_2-jdk@sha256:ce2ef9d2d67a68de5e196d4de9d743327c44a10b57fe02ec39f572c652cacfd3 AS java-env

# 1. システム依存パッケージ
RUN apt update && \
    apt install -y --no-install-recommends \
        libatomic1 curl jq git gh git-lfs maven openssh-client iputils-ping sudo && \
    apt clean && \
    rm -rf /var/lib/apt/lists/*

# 2. Node.js 環境整備
COPY --from=node-env /usr/local/bin/node /usr/local/bin/node
COPY --from=node-env /usr/local/lib/node_modules /usr/local/lib/node_modules
RUN apt update && apt install -y libatomic1 && \
    ln -s /usr/local/lib/node_modules/npm/bin/npm-cli.js /usr/local/bin/npm && \
    ln -s /usr/local/lib/node_modules/npm/bin/npx-cli.js /usr/local/bin/npx && \
    apt clean && \
    rm -rf /var/lib/apt/lists/*

# 3. 非 root ユーザー作成
ARG USERNAME=dev
ARG USER_UID=1000
ARG USER_GID=1000
RUN if id -u ${USER_UID} >/dev/null 2>&1; then userdel -r $(id -nu ${USER_UID}); fi && \
    if getent group ${USER_GID} >/dev/null 2>&1; then groupdel ${USER_GID}; fi && \
    groupadd --gid ${USER_GID} ${USERNAME} && \
    useradd --uid ${USER_UID} --gid ${USER_GID} --create-home --shell /bin/bash ${USERNAME} && \
    echo "${USERNAME} ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/${USERNAME} && \
    chmod 0440 /etc/sudoers.d/${USERNAME}

# 4. Antigravity CLI のインストール
USER ${USERNAME}
RUN curl -fsSL --compressed https://antigravity.google/cli/install.sh | bash
USER root
RUN ln -s /home/${USERNAME}/.local/bin/agy /usr/local/bin/agy
