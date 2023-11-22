ARG PHP_QUEUE_VERSION=7.4

FROM php:${PHP_QUEUE_VERSION}-alpine

LABEL maintainer="Think <think@thinks.fun>"

# Set Timezone
# https://wiki.alpinelinux.org/wiki/Setting_the_timezone
ARG TIME_ZONE=Asia/Shanghai
RUN apk --no-cache add tzdata && \
    cp "/usr/share/zoneinfo/${TIME_ZONE}" /etc/localtime && \
    echo "${TIME_ZONE}" > /etc/timezone && \
    apk del tzdata

RUN apk --update add build-base \
  libmemcached-dev \
  libmcrypt-dev \
  libxml2-dev \
  oniguruma-dev \
  pcre-dev \
  zlib-dev \
  autoconf \
  cyrus-sasl-dev \
  libgsasl-dev \
  shadow

RUN pecl channel-update pecl.php.net && pecl install mcrypt-1.0.3 memcached redis && docker-php-ext-enable mcrypt memcached redis

RUN docker-php-ext-install mysqli mbstring pdo pdo_mysql tokenizer xml pcntl


RUN rm /var/cache/apk/* \
    && usermod -u 1000 www-data

WORKDIR /var/www/html/
USER www-data