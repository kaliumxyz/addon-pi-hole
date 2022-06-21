ARG BUILD_FROM=ghcr.io/hassio-addons/base/amd64:12.0.0
FROM ${BUILD_FROM}

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

#!/bin/bash

# https://github.com/pi-hole/docker-pi-hole/blob/master/README.md

ENV PATH="${PATH}:/opt/pihole" \
    CORE_TAG="v5.10" \
    WEB_TAG="v5.12" \
    FTL_TAG="v5.15"


# We need to copy in the patches need during build
COPY rootfs/patches /patches

# Setup base
# hadolint ignore=DL3003
RUN \
    apk add --no-cache --virtual .build-dependencies \
        gcc=11.2.1_git20220219-r2 \
        make=4.3-r0 \
        cmake=3.23.1-r0 \
        musl-dev=1.2.3-r0 \
        libexecinfo-dev=1.1-r1 \
        linux-headers=5.16.7-r1 \
        nettle-dev=3.7.3-r0 \
        build-base=0.5-r3 \
        libidn=1.38-r0 \
    \
    && apk add --no-cache \
        php8=8.0.20-r0 \
        php8-cgi=8.0.20-r0 \
        php8-sqlite3=8.0.20-r0 \
        php8-session=8.0.20-r0 \
        php8-openssl=8.0.20-r0 \
        php8-json \
        php8-fileinfo=8.0.20-r0 \
        php8-phar=8.0.20-r0 \
        bc=1.07.1-r1 \
        bind-tools=9.16.29-r0 \
        coreutils=9.1-r0 \
        git=2.36.1-r0 \
        grep=3.7-r0 \
        libcap=2.64-r0 \
        libxml2=2.9.14-r0 \
        logrotate=3.19.0-r1 \
        lua-resty-http=0.16.1-r0 \
        ncurses=6.3_p20220521-r0 \
        nettle=3.7.3-r0 \
        nginx-mod-http-lua=1.22.0-r0 \
        nginx=1.22.0-r0 \
        openssl=1.1.1o-r0 \
        perl=5.34.1-r0 \
        procps=3.3.17-r1 \
        psmisc=23.5-r0 \
        sed=4.8-r0 \
        sqlite=3.38.5-r0 \
        sudo=1.9.10-r0 \
        wget=1.21.3-r0 \
fd \
    \
    && addgroup -S pihole \
    && adduser -S -s /sbin/nologin pihole pihole \
    && addgroup pihole nginx \
    \
    && git clone --branch "${CORE_TAG}" --depth=1 \
        https://github.com/pi-hole/pi-hole.git /etc/.pihole \
    && git -C /etc/.pihole checkout -b master \
    \
    && git clone --branch "${WEB_TAG}" --depth=1 \
        https://github.com/pi-hole/AdminLTE.git /var/www/html/admin \
    && git -C /var/www/html/admin checkout -b master \
    \
    && git clone --branch "${FTL_TAG}" --depth=1 \
        https://github.com/pi-hole/FTL.git /root/FTL \
    && git -C /root/FTL checkout -b master \
    \
    && cd /root/FTL \
    && ls /\
    && patch -p1 < /patches/FTL/libidn.patch \
    && ln -s /usr/lib/libidn.so.12 ./libidn.so \
    && ./build.sh -i \
    && mv /root/FTL/pihole-FTL /usr/bin \
    && cd - \
    \
    && install -o pihole -Dm755 -d "/opt/pihole" \
    && install -o pihole -Dm755 -t "/opt/pihole" /etc/.pihole/gravity.sh \
    && install -o pihole -Dm755 -t "/opt/pihole" /etc/.pihole/advanced/Scripts/*.sh \
    && install -o pihole -Dm755 -t "/opt/pihole" /etc/.pihole/advanced/Scripts/COL_TABLE \
    \
    && cd /etc/.pihole/ \
    && install -o pihole -Dm755 -t "/usr/local/bin" /etc/.pihole/pihole \
    \
    && apk del --purge .build-dependencies \
    && rm -fr \
        /etc/nginx \
        /root/FTL

# Copy root filesystem
COPY rootfs /

# Build arguments
ARG BUILD_ARCH
ARG BUILD_DATE
ARG BUILD_REF
ARG BUILD_VERSION

# Labels
LABEL \
    io.hass.name="Pi-hole" \
    io.hass.description="Network-wide ad blocking" \
    io.hass.arch="${BUILD_ARCH}" \
    io.hass.type="addon" \
    io.hass.version=${BUILD_VERSION} \
    maintainer="kaliumxyz <kalium@kalium.xyz>" \
    org.label-schema.description="Network-wide ad blocking" \
    org.label-schema.build-date=${BUILD_DATE} \
    org.label-schema.name="Pi-hole" \
    org.label-schema.schema-version="1.1" \
    org.label-schema.usage="https://github.com/kaliumxyz/addon-pi-hole/master/readme.md" \
    org.label-schema.vcs-ref=${BUILD_REF} \
    org.label-schema.vcs-url="https://github.com/kaliumxyz/addon-pi-hole" \
    org.label-schema.vendor="Home Assistant Community Add-ons"
