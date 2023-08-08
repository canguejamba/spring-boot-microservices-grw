FROM mcr.microsoft.com/devcontainers/java:1-17-bullseye

ARG USERNAME
ARG USER_GROUP_NAME
ARG USER_UID
ARG USER_GID

# general packages
ARG PKG="sudo git vim curl zip unzip locales ${TMP_PKG}"

SHELL ["/bin/bash", "-c"]

RUN apt-get update \
    && apt-get install -y ${PKG} \
    && groupadd --gid ${USER_GID} ${USER_GROUP_NAME} \
    && useradd --uid ${USER_UID} --shell /bin/bash --gid ${USER_GID} -m ${USERNAME} \
    && echo %${USER_GROUP_NAME} ALL=\(ALL\) NOPASSWD:ALL > /etc/sudoers.d/${USER_GROUP_NAME} \
    && chmod 0440 /etc/sudoers.d/${USER_GROUP_NAME}

# Set the working directory
WORKDIR /home/cganda/workspace

# enable git
RUN su ${USERNAME} --command \
    'sudo git config --system --add safe.directory "/home/${USERNAME}/workspace"'

ARG INSTALL_MAVEN="true"
ARG MAVEN_VERSION=""

ARG INSTALL_GRADLE="false"
ARG GRADLE_VERSION=""

RUN if [ "${INSTALL_MAVEN}" = "true" ]; then su vscode -c "umask 0002 && . /usr/local/sdkman/bin/sdkman-init.sh && sdk install maven \"${MAVEN_VERSION}\""; fi \
    && if [ "${INSTALL_GRADLE}" = "true" ]; then su vscode -c "umask 0002 && . /usr/local/sdkman/bin/sdkman-init.sh && sdk install gradle \"${GRADLE_VERSION}\""; fi

