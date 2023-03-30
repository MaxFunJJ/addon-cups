ARG BUILD_FROM=ghcr.io/hassio-addons/base:13.1.5
FROM ${BUILD_FROM}

LABEL io.hass.version="1" io.hass.type="addon" io.hass.arch="armhf|aarch64|i386|amd64"

RUN apk add --no-cache --virtual .build-dependencies \
        cups \
        avahi-daemon \
        libnss-mdns \
        dbus \
        colord \
        printer-driver-all \
        printer-driver-gutenprint \
        openprinting-ppds \
        hpijs-ppds \
        hp-ppd  \
        hplip \
        printer-driver-foo2zjs \
        gnupg2 \
        lsb-release \
        nano \
        samba \
        bash-completion \
        nginx \
    && apk del --no-cache --purge .build-dependencies \
    && rm -rf /var/lib/apt/lists/*

COPY rootfs /

# Health check
HEALTHCHECK --start-period=10m \
    CMD curl --fail http://127.0.0.1:46836 || exit 1

# Corrects permissions for s6 v3
RUN if [ -d /etc/cont-init.d ]; then chmod -R 755 /etc/cont-init.d; fi && \
    if [ -d /etc/services.d ]; then chmod -R 755 /etc/services.d; fi && \
    if [ -f /entrypoint.sh ]; then chmod 755 /entrypoint.sh; fi

EXPOSE 631 445 137 139

RUN chmod a+x /run.sh
CMD ["/run.sh"]
