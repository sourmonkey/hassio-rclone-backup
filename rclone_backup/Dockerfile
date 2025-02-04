ARG BUILD_FROM
FROM $BUILD_FROM

# Build arguments
ARG BUILD_ARCH
ARG BUILD_NAME
ARG BUILD_DESCRIPTION
ARG BUILD_REPOSITORY
ARG BUILD_SHA

ENV LANG C.UTF-8
ENV RCLONE_INSTALLED_VERSION 1.56.0

# Copy root filesystem
COPY rootfs /

# Install requirements for add-on
RUN if [ "${BUILD_ARCH}" = "armhf" ]; then \
        curl -LJo /tmp/rclone.zip https://github.com/rclone/rclone/releases/download/v"${RCLONE_INSTALLED_VERSION}"/rclone-v"${RCLONE_INSTALLED_VERSION}"-linux-arm.zip; \
    elif [ "${BUILD_ARCH}" = "armv7" ]; then \
        curl -LJo /tmp/rclone.zip https://github.com/rclone/rclone/releases/download/v"${RCLONE_INSTALLED_VERSION}"/rclone-v"${RCLONE_INSTALLED_VERSION}"-linux-arm-v7.zip; \
    elif [ "${BUILD_ARCH}" = "aarch64" ]; then \
        curl -LJo /tmp/rclone.zip https://github.com/rclone/rclone/releases/download/v"${RCLONE_INSTALLED_VERSION}"/rclone-v"${RCLONE_INSTALLED_VERSION}"-linux-arm64.zip; \
    elif [ "${BUILD_ARCH}" = "i386" ]; then \
        curl -LJo /tmp/rclone.zip https://github.com/rclone/rclone/releases/download/v"${RCLONE_INSTALLED_VERSION}"/rclone-v"${RCLONE_INSTALLED_VERSION}"-linux-386.zip; \
    elif [ "${BUILD_ARCH}" = "amd64" ]; then \
        curl -LJo /tmp/rclone.zip https://github.com/rclone/rclone/releases/download/v"${RCLONE_INSTALLED_VERSION}"/rclone-v"${RCLONE_INSTALLED_VERSION}"-linux-amd64.zip; \
    else \
        exit 1; \
    fi \
    && unzip /tmp/rclone.zip -d /tmp \
    && cp /tmp/rclone-*/rclone /usr/bin/rclone  \
    && chmod 755 /usr/bin/rclone \
    && rm -fr /tmp/rclone* \
    && mkdir -p /root/.config/rclone \
    && pip install --no-cache-dir python-slugify

# Labels
LABEL \
    org.opencontainers.image.title="${BUILD_NAME}" \
    org.opencontainers.image.description="${BUILD_DESCRIPTION}" \
    org.opencontainers.image.licenses="MIT" \
    org.opencontainers.image.url="https://github.com/${BUILD_REPOSITORY}" \
    org.opencontainers.image.source="https://github.com/${BUILD_REPOSITORY}" \
    org.opencontainers.image.documentation="https://github.com/${BUILD_REPOSITORY}/blob/main/README.md" \
    org.opencontainers.image.revision=${BUILD_SHA} \
    org.opencontainers.image.authors="" \
    org.opencontainers.image.vendor="" \
    maintainer=""
