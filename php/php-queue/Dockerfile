ARG PHP_VERSION=7.3
FROM php:${PHP_VERSION}-alpine

LABEL maintainer="Think <think@thinks.fun>"

RUN apk --update add build-base \
  libmemcached-dev \
  libmcrypt-dev \
  libxml2-dev \
  pcre-dev \
  zlib-dev \
  autoconf \
  cyrus-sasl-dev \
  libgsasl-dev \
  shadow


RUN docker-php-ext-install mysqli mbstring pdo pdo_mysql tokenizer xml pcntl

RUN pecl channel-update pecl.php.net && pecl install memcached redis && docker-php-ext-enable memcached redis

RUN rm /var/cache/apk/* \
    && usermod -u 1000 www-data

WORKDIR /var/www/html/