FROM mcr.microsoft.com/devcontainers/base:jammy

ARG PIXI_VERSION=v0.48.2
ARG PIXI_ENV=default

RUN curl -L -o /usr/local/bin/pixi -fsSL --compressed "https://github.com/prefix-dev/pixi/releases/download/${PIXI_VERSION}/pixi-$(uname -m)-unknown-linux-musl" \
    && chmod +x /usr/local/bin/pixi \
    && pixi info

# set some user and workdir settings to work nicely with vscode
ENV VSCODE_HOME=/home/vscode \
    VSCODE_USER=vscode \
    PIXI_ENV=${PIXI_ENV} \
    SHELL=/bin/bash

USER vscode

RUN echo 'eval "$(pixi completion -s bash)"' >> ${VSCODE_HOME}/.bashrc

# Create the shell-hook bash script to activate the environment
RUN pixi shell-hook -e ${PIXI_ENV} > ${VSCODE_HOME}/shell-hook.sh

# extend the shell-hook script to run the command passed to the container
RUN echo 'exec "$@"' >> ${VSCODE_HOME}/shell-hook.sh

# set the entrypoint to the shell-hook script (activate the environment and run the command)
# no more pixi needed in the prod container
ENTRYPOINT ["/bin/bash", "${VSCODE_HOME}/shell-hook.sh"]
