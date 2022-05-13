# [Choice] Java version (use -bullseye variants on local arm64/Apple Silicon): 11, 17, 11-bullseye, 17-bullseye, 11-buster, 17-buster
ARG VARIANT=11-bullseye
FROM mcr.microsoft.com/vscode/devcontainers/java:0-${VARIANT}

# [Option] Install Maven
ARG INSTALL_MAVEN="false"
ARG MAVEN_VERSION=""
# [Option] Install Gradle
ARG INSTALL_GRADLE="false"
ARG GRADLE_VERSION=""
RUN if [ "${INSTALL_MAVEN}" = "true" ]; then su vscode -c "umask 0002 && . /usr/local/sdkman/bin/sdkman-init.sh && sdk install maven \"${MAVEN_VERSION}\""; fi \
    && if [ "${INSTALL_GRADLE}" = "true" ]; then su vscode -c "umask 0002 && . /usr/local/sdkman/bin/sdkman-init.sh && sdk install gradle \"${GRADLE_VERSION}\""; fi

# [Choice] Node.js version: none, lts/*, 16, 14, 12, 10
ARG NODE_VERSION="none"
RUN if [ "${NODE_VERSION}" != "none" ]; then su vscode -c "umask 0002 && . /usr/local/share/nvm/nvm.sh && nvm install ${NODE_VERSION} 2>&1"; fi

ARG HTTP_PROXY="none"
ARG NO_PROXY="*"
RUN if [ "${HTTP_PROXY}" != "none" ]; then echo "proxy=\"${HTTP_PROXY}\"\n\
https-proxy=\"${HTTP_PROXY}\"\n\
strict-ssl=false" > /home/vscode/.npmrc; fi

ENV http_proxy=${HTTP_PROXY}
ENV HTTP_PROXY=${HTTP_PROXY}
ENV https_proxy=${HTTP_PROXY}
ENV HTTPS_PROXY=${HTTP_PROXY}
ENV no_proxy=${NO_PROXY}
ENV NO_PROXY=${NO_PROXY}

ARG NPM_MIRROR="none"
RUN if [ "${NPM_MIRROR}" != "none" ]; then echo "registry=\"${NPM_MIRROR}\"" >> /home/vscode/.npmrc; fi

# [Optional] Uncomment this line to install global node packages.
RUN su vscode -c "source /usr/local/share/nvm/nvm.sh && npm install -g npm@latest && npm install -g generator-jhipster" 2>&1

ARG APT_MIRROR="none"
RUN if [ "${APT_MIRROR}" != "none" ]; then sed -i "s,http://deb.debian.org,\"${APT_MIRROR}\",g" /etc/apt/sources.list \
    && sed -i "s,http://security.debian.org,\"${APT_MIRROR}\",g" /etc/apt/sources.list; fi

RUN if [ "${HTTP_PROXY}" != "none" ]; then echo "Acquire::http::Proxy \"${HTTP_PROXY}\";\n\
Acquire::https::Proxy \"${HTTP_PROXY}\";\n\
Acquire::https::Proxy::\"${NO_PROXY}\" DIRECT;" > /etc/apt/apt.conf.d/99proxy.conf; fi \
    && cat /etc/apt/apt.conf.d/99proxy.conf

# [Optional] Uncomment this section to install additional OS packages.
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends tree

RUN su vscode -c "umask 0002 && . /usr/local/sdkman/bin/sdkman-init.sh && sdk install springboot"

RUN su vscode -c "umask 0002 && . /usr/local/sdkman/bin/sdkman-init.sh && sdk install java 22.0.0.2.r17-grl"

RUN export DEBIAN_FRONTEND=noninteractive \
    && apt upgrade -y \
    && apt-get -y install zsh-autosuggestions zsh-syntax-highlighting \
    && echo "source /usr/share/zsh-autosuggestions/zsh-autosuggestions.zsh" >> /home/vscode/.zshrc \
    && echo "source /usr/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh" >> /home/vscode/.zshrc