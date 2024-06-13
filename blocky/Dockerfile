ARG BUILD_FROM=ghcr.io/hassio-addons/base:16.0.1
# hadolint ignore=DL3006
FROM ${BUILD_FROM}

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Define the blocky version
ARG BLOCKY_VERSION=0.24

# Build arguments
ARG BUILD_ARCH
ARG BUILD_DATE
ARG BUILD_DESCRIPTION
ARG BUILD_NAME
ARG BUILD_REF
ARG BUILD_REPOSITORY
ARG BUILD_VERSION

# Set the appropriate download URL based on the architecture
# hadolint ignore=DL3003,DL3018
RUN case ${BUILD_ARCH} in \
      "i386") \
        ARCH="x86_64" ;; \
      "amd64") \
        ARCH="x86_64" ;; \
      "armhf") \
        ARCH="armv6" ;; \
      "armv7") \
        ARCH="armv7" ;; \
      "aarch64") \
        ARCH="arm64" ;; \
      *) \
        echo "Unsupported architecture: ${BUILD_ARCH}" && exit 1 ;; \
    esac && \
    echo "Downloading Blocky for architecture: ${ARCH}" && \
    apk add --no-cache wget && \
    wget -q -O /tmp/blocky.tar.gz "https://github.com/0xERR0R/blocky/releases/download/v${BLOCKY_VERSION}/blocky_v${BLOCKY_VERSION}_Linux_${ARCH}.tar.gz" && \
    tar -xzf /tmp/blocky.tar.gz -C /usr/local/bin/ && \
    rm /tmp/blocky.tar.gz && \
    chmod +x /usr/local/bin/blocky

# Copy root filesystem
COPY rootfs/ /

# Labels
LABEL \
  maintainer="Fabian Pahl <hi@robocopklaus.dev>" \
  io.hass.name="${BUILD_NAME}" \
  io.hass.description="${BUILD_DESCRIPTION}" \
  io.hass.arch="${BUILD_ARCH}" \
  io.hass.type="addon" \
  io.hass.version=${BUILD_VERSION} \
  io.hass.name="${BUILD_NAME}" \
  io.hass.description="${BUILD_DESCRIPTION}" \
  org.opencontainers.image.authors="Fabian Pahl <hi@robocopklaus.dev>" \
  org.opencontainers.image.licenses="MIT" \
  org.opencontainers.image.source="https://github.com/${BUILD_REPOSITORY}" \
  org.opencontainers.image.documentation="https://github.com/${BUILD_REPOSITORY}/blob/main/README.md" \
  org.opencontainers.image.created=${BUILD_DATE} \
  org.opencontainers.image.revision=${BUILD_REF} \
  org.opencontainers.image.version=${BUILD_VERSION}