ARG BUILD_FROM=ghcr.io/hassio-addons/base/aarch64:18.0.0

FROM ${BUILD_FROM}

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Build Nebula 2
RUN \
    apk add --no-cache musl=1.2.5-r12 musl-dev=1.2.5-r12 \
    \
    && apk add --no-cache --virtual .build-dependencies \
        build-base \
        git \
    \
    && apk add --no-cache \
        iptables \
        yq \
        bind-tools \
    \
    && wget -qO /tmp/go.tar.gz https://go.dev/dl/go1.25.0.linux-arm64.tar.gz \
    && tar -C /usr/local -xzf /tmp/go.tar.gz \
    && export PATH=$PATH:/usr/local/go/bin \
    \
    && git clone --branch "v1.10.3" --depth=1 \
        "https://github.com/slackhq/nebula.git" /tmp/nebula \
    \
    && cd /tmp/nebula \
    && make bin \
    && mv nebula /usr/bin/nebula \
    && mv nebula-cert /usr/bin/nebula-cert \
#    && chmod +x /usr/bin/nebula \
#    && chmod +x /usr/bin/nebula-cert \
    \
    && rm -f -r /tmp/* \
    && apk del --no-cache --purge \
        .build-dependencies \
        go

COPY rootfs /
