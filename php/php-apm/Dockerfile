ARG PHP_WORKER_VERSION=php-alpine

FROM thinksvip/lnmp:${PHP_WORKER_VERSION}

LABEL maintainer="Think <think@thinks.fun>"

# Install general dependencies
RUN apk --update add \
  curl-dev \
  libevent-dev \
  libmcrypt-dev \
  libxml2-dev \
  pcre-dev \
  g++ \
  autoconf \
  make \
  shadow \
  oniguruma-dev \
  libwebp-dev \
  llvm13-dev \
  clang-dev \
  protobuf-c-compiler \
  protobuf-dev

# INSTALL_SKYWALKING
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- --default-toolchain none -y &&\
    source $HOME/.cargo/env &&\
    export RUSTFLAGS="-C target-feature=-crt-static" &&\
    pecl install skywalking_agent &&\
    docker-php-ext-enable skywalking_agent 

# Clean up
RUN apk del autoconf g++ make libwebp-dev llvm13-dev clang-dev protobuf-c-compiler protobuf-dev .phpize-deps && apk del --purge && rm -rf /tmp/* /var/cache/apk/*