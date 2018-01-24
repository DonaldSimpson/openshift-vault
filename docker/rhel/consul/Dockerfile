FROM registry.access.redhat.com/rhel7
MAINTAINER Ric Featherstone <ricfeatherstone@users.noreply.github.com>

ENV CONSUL_VERSION=1.0.2 \
    DUMB_INIT_VERSION=1.2.1 \
    INSTALL_PKGS="unzip"

LABEL io.k8s.description="" \
      io.k8s.display-name="" \
      io.openshift.expose-services="8443:http" \
      io.openshift.tags=""

RUN groupadd consul -g 5000 && \
    useradd -g consul -r -M -u 1000 consul && \
    mkdir -p /consul/data && \
    mkdir -p /consul/config && \
    chown -R consul:consul /consul

VOLUME /consul/data

EXPOSE 8300 8301 8301/udp 8302 8302/udp 8443 8500 8600 8600/udp

RUN yum -y --setopt=tsflags=nodocs install $INSTALL_PKGS && \
    yum clean all && \
    rm -rf /var/cache/yum && \
    curl -sSL -o /tmp/consul.zip "https://releases.hashicorp.com/consul/${CONSUL_VERSION}/consul_${CONSUL_VERSION}_linux_amd64.zip" && \
    unzip -qq /tmp/consul.zip -d /usr/bin/ && \
    rm /tmp/consul.zip && \
    curl -sSL -o /usr/bin/dumb-init "https://github.com/Yelp/dumb-init/releases/download/v${DUMB_INIT_VERSION}/dumb-init_${DUMB_INIT_VERSION}_amd64" && \
    chmod +x /usr/bin/dumb-init

USER 1000

ENTRYPOINT ["/usr/bin/dumb-init", "consul"]
CMD ["agent", "-dev", "-client", "0.0.0.0"]
