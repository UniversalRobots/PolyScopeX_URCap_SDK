FROM node:18.17.1-bookworm
ENV PS_VERSION=0.12.125 \
    URSIM_VERSION=0.10.147 \
    SDK_VERSION=5.1.45
SHELL ["/bin/bash", "-c"]

RUN set -ex; \
    apt-get update && apt-get install -y \
             build-essential \
             ca-certificates \
             curl \
             gnupg \
             sudo \
             tzdata \
             unzip \
             rsync; \
    rm -rf /var/lib/apt/lists/*

#   Install docker
RUN set -ex; \
    curl -fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg; \
    echo \
    "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian \
    $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null; \
    apt-get update -y; \
    apt-get install -y \
        docker-ce \
        docker-ce-cli \
        containerd.io  \
        docker-buildx-plugin \
        docker-compose-plugin \
        gh; \
    rm -rf /var/lib/apt/lists/* 

#   Copy zipped files to /
COPY *.zip /

#   Set up non-root user
ARG USERNAME=psxdev
ARG USER_UID=1001
ARG USER_GID=$USER_UID
RUN set -ex; \
    groupadd --gid $USER_GID $USERNAME; \
    useradd --uid $USER_UID --gid $USER_GID -m $USERNAME; \
    usermod -aG docker $USERNAME; \
    echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME; \
    chmod 0440 /etc/sudoers.d/$USERNAME

#   unzip files, chown to psxdev
RUN set -ex; \
    unzip '*.zip'; \
    rm *.zip; \
    chown -R $USERNAME:$USERNAME sdk-polyscopex-${SDK_VERSION}/ ursim-polyscopex-${URSIM_VERSION}/  /usr/local/lib/node_modules/

USER $USERNAME

RUN set -ex; \
    pushd /ursim-polyscopex-${URSIM_VERSION} > /dev/null; \
    npm run build-local --prefix artifacts/runtime; \
    popd > /dev/null

RUN set -ex; \
	pushd /sdk-polyscopex-${SDK_VERSION}/urcap-generator > /dev/null; \
	npm install --no-update-notifier ; \
	npm link; \
	cp -f sh_newurcap ../newurcap.sh; \
	npm config set @universal-robots:registry https://pkgs.dev.azure.com/polyscopex/api/_packaging/polyscopex/npm/registry/; \
	npm config set strict-ssl false; \
	popd > /dev/null