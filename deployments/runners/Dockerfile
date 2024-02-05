FROM ubuntu:latest

ARG R_USER="runner_user"
ARG R_HOME="/home/${R_USER}"

RUN set -eux \
    && DEBIAN_FRONTEND=noninteractive apt-get --yes update \
    && DEBIAN_FRONTEND=noninteractive apt-get install --yes \
        sudo \
        curl \
        python3 \
    # CREATE USER
    && adduser \
        --home ${R_HOME} \
        --disabled-password \
        --disabled-login \
        ${R_USER} \
    && usermod -aG sudo ${R_USER} \
    && echo "${R_USER}  ALL=(ALL:ALL) NOPASSWD:ALL" > /etc/sudoers.d/runner

# USER ------------

USER ${R_USER}

ARG GH_PAT
ARG SRC_PKG="actions-runner-linux-x64-2.312.0.tar.gz"

ARG WORKDIR="${R_HOME}/actions-runner"
WORKDIR ${WORKDIR}

# Download
RUN set -eux \
    && curl -o ${SRC_PKG} -L https://github.com/actions/runner/releases/download/v2.312.0/${SRC_PKG} \
    && tar xzf ./${SRC_PKG} \
    && sudo ./bin/installdependencies.sh \
    # CLEAN INSTALLATION
    && rm -v ${SRC_PKG} \
    && DEBIAN_FRONTEND=noninteractive sudo apt-get autoremove --yes \
    && DEBIAN_FRONTEND=noninteractive sudo apt-get clean \
    && sudo rm -fr /var/lib/apt/lists/* \
        /tmp/* \
        /var/tmp/* \ 
        ~/.cache 

# Configure
RUN set -eux \
    # Create the runner and start the configuration experience
    && ./config.sh \
        --unattended \
        --name my-runner \
        --url https://github.com/marjau/gh-advanced-lkn \
        --token ALPCRUB6Y5I44GUH4RKS3Y3FYFE22

ENTRYPOINT [ "./run.sh" ] 
    
# Using your self-hosted runner
# # Use this YAML in your workflow file for each job
# runs-on: self-hosted