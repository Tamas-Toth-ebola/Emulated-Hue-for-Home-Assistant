ARG HASS_ARCH=amd64
ARG BUILD_VERSION=latest
ARG PYTHON_VERSION="3.11"
ARG BASHIO_VERSION="v0.15.0"
ARG S6_OVERLAY_VERSION="3.1.6.2"

#####################################################################
#                                                                   #
# Download and extract s6 overlay & bashio                          #
#                                                                   #
#####################################################################
FROM alpine:latest AS downloader
ARG HASS_ARCH
ARG S6_OVERLAY_VERSION
ARG BASHIO_VERSION

RUN apk add --no-cache curl tar xz

# Get s6-overlay
RUN set -x \
    && if [ "${HASS_ARCH}" = "i386" ]; then S6_ARCH="i686"; fi \
    && if [ "${HASS_ARCH}" = "amd64" ]; then S6_ARCH="x86_64"; fi \
    && if [ "${HASS_ARCH}" = "armv6" ]; then S6_ARCH="arm"; fi \
    && if [ "${HASS_ARCH}" = "armv7" ]; then S6_ARCH="armhf"; fi \
    && if [ "${HASS_ARCH}" = "aarch64" ]; then S6_ARCH="aarch64"; fi \
    && curl -L -o /tmp/s6-noarch.tar.xz "https://github.com/just-containers/s6-overlay/releases/download/v${S6_OVERLAY_VERSION}/s6-overlay-noarch.tar.xz" \
    && curl -L -o /tmp/s6-arch.tar.xz "https://github.com/just-containers/s6-overlay/releases/download/v${S6_OVERLAY_VERSION}/s6-overlay-${S6_ARCH}.tar.xz" \
    && mkdir -p /s6 \
    && tar -Jxvf /tmp/s6-noarch.tar.xz -C /s6 \
    && tar -Jxvf /tmp/s6-arch.tar.xz -C /s6

# Get bashio
RUN curl -L "https://github.com/hassio-addons/bashio/archive/${BASHIO_VERSION}.tar.gz" | tar xz --strip 1 -C /tmp \
    && mkdir -p /bashio/usr/lib/bashio /bashio/usr/bin \
    && mv /tmp/lib/* /bashio/usr/lib/bashio/ \
    && ln -s /usr/lib/bashio/bashio /bashio/usr/bin/bashio

#####################################################################
#                                                                   #
# Final Image                                                       #
#                                                                   #
#####################################################################
FROM python:${PYTHON_VERSION}-slim-bookworm AS final
WORKDIR /app

# Use piwheels for pre-built binaries on ARM to avoid freezing the Pi
ENV PIP_EXTRA_INDEX_URL=https://www.piwheels.org/simple \
    DEBIAN_FRONTEND="noninteractive" \
    S6_BEHAVIOUR_IF_STAGE2_FAILS=2 \
    S6_CMD_WAIT_FOR_SERVICES_MAXTIME=0

RUN apt-get update && apt-get install -y --no-install-recommends \
    ca-certificates \
    curl \
    git \
    jq \
    openssl \
    tzdata \
    && rm -rf /var/lib/apt/lists/*

# Copy system tools from downloader
COPY --from=downloader /s6 /
COPY --from=downloader /bashio /

# Install python dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy root filesystem
COPY --chmod=0755 docker/rootfs /

# Copy app
COPY emulated_hue emulated_hue

# Metadata
ARG BUILD_VERSION
ARG HASS_ARCH
LABEL \
    io.hass.version=${BUILD_VERSION} \
    io.hass.name="HA Emulated Hue" \
    io.hass.description="Hue Emulation for Home Assistant" \
    io.hass.arch="${HASS_ARCH}" \
    io.hass.type="addon"

CMD ["/init"]
