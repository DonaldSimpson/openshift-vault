FROM centos
MAINTAINER Ric Featherstone <ricfeatherstone@users.noreply.github.com>

ENV VAULT_VERSION=0.9.1 \
    DUMB_INIT_VERSION=1.2.1 \
    INSTALL_PKGS="unzip"

LABEL io.k8s.description="" \
      io.k8s.display-name="" \
      io.openshift.expose-services="8443:http" \
      io.openshift.tags=""

RUN groupadd vault -g 5000 && \
    useradd -g vault -r -M -u 1000 vault && \
    mkdir -p /vault/logs && \
    mkdir -p /vault/file && \
    mkdir -p /vault/config && \
    chown -R vault:vault /vault

VOLUME /vault/logs

VOLUME /vault/file

EXPOSE 8443

RUN yum -y --setopt=tsflags=nodocs install $INSTALL_PKGS && \
    yum clean all && \
    rm -rf /var/cache/yum && \
    curl -sSL -o /tmp/vault.zip "https://releases.hashicorp.com/vault/${VAULT_VERSION}/vault_${VAULT_VERSION}_linux_amd64.zip" && \
    unzip -qq /tmp/vault.zip -d /usr/bin/ && \
    rm /tmp/vault.zip && \
    curl -sSL -o /usr/bin/dumb-init "https://github.com/Yelp/dumb-init/releases/download/v${DUMB_INIT_VERSION}/dumb-init_${DUMB_INIT_VERSION}_amd64" && \
    chmod +x /usr/bin/dumb-init && \
    setcap cap_ipc_lock=+ep /usr/bin/vault

USER 1000

ENTRYPOINT ["/usr/bin/dumb-init", "vault"]
CMD ["server", "-dev"]
