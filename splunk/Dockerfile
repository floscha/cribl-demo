FROM cribl/splunk:8.0.0
ARG CRIBL_DISTRO=cribl-splunk-app
RUN sh -c 'echo dash dash/sh boolean false | debconf-set-selections' && \
    sh -c 'DEBIAN_FRONTEND=noninteractive dpkg-reconfigure dash' && \
    apt-get update && \
    apt-get install -y vim nano curl ca-certificates jq && \
    rm -rf /var/lib/apt/lists/*
RUN VERSION=$(curl -s https://cdn.cribl.io/versions.json | jq -r .version) && \
    curl -Lso /tmp/${CRIBL_DISTRO}.tgz https://cdn.cribl.io/dl/$(echo ${VERSION} | cut -d '-' -f 1)/${CRIBL_DISTRO}-${VERSION}-linux-x64.tgz && \
    tar -zxvf /tmp/${CRIBL_DISTRO}.tgz -C /var/opt/splunk/etc/apps && \
    rm /tmp/${CRIBL_DISTRO}.tgz
COPY hack/splunk/etc /var/opt/splunk/etc
COPY config.sh /var/opt/splunk/etc/apps/cribl/bin/config.sh
COPY cribldemo/ /var/opt/splunk/etc/apps/cribl/
ENV SPLUNK_USER root
ENV SPLUNK_START_ARGS "--accept-license --answer-yes --seed-passwd cribldemo"
ENV SPLUNK_BEFORE_START_CMD "version ${SPLUNK_START_ARGS}"
ENV SPLUNK_BEFORE_START_CMD_1 "cmd /bin/bash /opt/splunk/etc/apps/cribl/bin/config.sh"
