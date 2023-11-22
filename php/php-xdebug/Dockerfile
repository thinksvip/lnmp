ARG PHP_WORKER_VERSION=php-alpine

FROM thinksvip/lnmp:${PHP_WORKER_VERSION}

LABEL maintainer="Think <think@thinks.fun>"

RUN apk add --no-cache --update --virtual .phpize-deps autoconf g++ make libwebp-dev

# INSTALL_XDEBUG
#  #Install the xdebug extension
RUN   if [ $(php -r "echo PHP_MAJOR_VERSION;") = "8" ] || { [ $(php -r "echo PHP_MAJOR_VERSION;") = "7" ] && { [ $(php -r "echo PHP_MINOR_VERSION;") = "4" ] || [ $(php -r "echo PHP_MINOR_VERSION;") = "3" ] ;} ;}; then \
    if [ $(php -r "echo PHP_MAJOR_VERSION;") = "8" ]; then \
      pecl install xdebug-3.2.1; \
    else \
      pecl install xdebug-3.1.6; \
    fi; \
  else \
    if [ $(php -r "echo PHP_MAJOR_VERSION;") = "5" ]; then \
      pecl install xdebug-2.5.5; \
    else \
      if [ $(php -r "echo PHP_MINOR_VERSION;") = "0" ]; then \
        pecl install xdebug-2.9.0; \
      else \
        pecl install xdebug-2.9.8; \
      fi \
    fi \
  fi && \
  docker-php-ext-enable xdebug 

  RUN apk del .phpize-deps autoconf g++ make libwebp-dev && apk del --purge && rm -rf /tmp/* /var/cache/apk/*