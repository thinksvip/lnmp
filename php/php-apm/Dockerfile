
# 第一阶段: 编译 skywalking-php
ARG PHP_WORKER_VERSION=php-alpine
FROM thinksvip/lnmp:${PHP_WORKER_VERSION} AS builder

# Install dependencies for building skywalking-php
RUN apk --update add \
    curl-dev \
    libevent-dev \
    libmcrypt-dev \
    libxml2-dev \
    pcre-dev \
    g++ \
    autoconf \
    make \
    oniguruma-dev \
    libwebp-dev \
    llvm13-dev \
    clang-dev \
    protobuf-c-compiler \
    protobuf-dev \
    git \
    ca-certificates && \
    update-ca-certificates

# Set environment variable to prevent git from prompting for credentials
ENV GIT_TERMINAL_PROMPT=0

# INSTALL_SKYWALKING
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- --default-toolchain none -y && \
    source $HOME/.cargo/env && \
    export RUSTFLAGS="-C target-feature=-crt-static" && \
    git clone --recursive https://github.com/apache/skywalking-php.git && \
    cd skywalking-php && \
    phpize && \
    ./configure && \
    make && \
    make install

RUN ls /usr/local/lib/php/extensions/no-debug-non-zts-* && \
    ls /usr/local/lib/php/extensions/no-debug-non-zts-*/skywalking_agent.so

# 第二阶段: 生产镜像，继承基础 lnmp 镜像
FROM thinksvip/lnmp:${PHP_WORKER_VERSION}

LABEL maintainer="Think <think@thinks.fun>"

# Copy skywalking extension from the builder stage
COPY --from=builder /usr/local/lib/php/extensions/no-debug-non-zts-*/skywalking_agent.so /tmp/

RUN mv /tmp/skywalking_agent.so $(php -r "echo ini_get('extension_dir');")/

# Enable the skywalking extension
RUN ls /usr/local/lib/php/extensions/no-debug-non-zts-* && \
    docker-php-ext-enable skywalking_agent