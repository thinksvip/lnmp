ARG PHP_FPM_VERSION=7.4

FROM php:${PHP_FPM_VERSION}-fpm-alpine

LABEL maintainer="Think <think@thinks.fun>"

# Extensions: ctype, dom, fileinfo, ftp, hash, iconv, json, pdo, pdo_sqlite, session,
# tokenizer, simplexml, xml, xmlreader, xmlwriter and phar are bundled and compiled into
# PHP by default. If missing, install them directly by `docker-php-ext-install extension_name`

# Notice:
# 1. Mcrypt was DEPRECATED in PHP 7.1.0, and REMOVED in PHP 7.2.0.
# 2. opcache requires PHP version >= 7.0.0.
# 3. soap requires libxml2-dev.
# 4. xml, xmlrpc, wddx require libxml2-dev and libxslt-dev.
# 5. Line `&& :\` is just for better reading and do nothing.

# Set Timezone
# https://wiki.alpinelinux.org/wiki/Setting_the_timezone
ARG TIME_ZONE=Asia/Shanghai
RUN apk --no-cache add tzdata && \
    cp "/usr/share/zoneinfo/${TIME_ZONE}" /etc/localtime && \
    echo "${TIME_ZONE}" > /etc/timezone && \
    apk del tzdata

# Install general dependencies
RUN apk --update add \
  curl \
  libevent-dev \
  libmcrypt-dev \
  libxml2-dev \
  pcre-dev \
  g++ \
  autoconf \
  make \
  shadow \ 
  oniguruma-dev \
  libwebp-dev 

# INSTALL_MCRYPT
RUN if [ $(php -r "echo PHP_MAJOR_VERSION;") = "7.4" ]; then \
    pecl install mcrypt-1.0.3 && docker-php-ext-enable mcrypt; \
    fi
# INSTALL_GD
RUN if [ $(php -r "echo PHP_MAJOR_VERSION;") = "7.4" ]; then \
        # "--with-xxx-dir" was removed from php 7.4,
        # issue: https://github.com/docker-library/php/issues/912
        options="--with-freetype --with-jpeg --with-webp"; \
    else \
        options="--with-gd --with-freetype-dir=/usr/include/ --with-png-dir=/usr/include/ --with-jpeg-dir=/usr/include/ --with-webp-dir=/usr/include/"; \
    fi 
RUN apk add --no-cache freetype-dev libjpeg-turbo-dev libpng-dev && \
    docker-php-ext-configure gd ${options} && \
    docker-php-ext-install gd && \
# INSTALL_INTL
    # Install intl and requirements
    apk add --no-cache icu-dev && \
    docker-php-ext-install intl && \
    # Install the soap extension
    docker-php-ext-install soap && \
# INSTALL_XSL
	apk add --no-cache libxslt-dev && \
    docker-php-ext-install xsl && \
# INSTALL_XMLRPC
	apk add --no-cache libxslt-dev && \
    docker-php-ext-install xmlrpc && \
# INSTALL_BZ2
    apk add --no-cache bzip2-dev && \
    docker-php-ext-install bz2 && \
# INSTALL_ZIP
    apk --update add libzip-dev zip && \
    docker-php-ext-configure zip --with-zip && \
    # Install the zip extension
    docker-php-ext-install zip && \
# INSTALL_PCNTL
    # Installs pcntl, helpful for running Horizon
    docker-php-ext-install pcntl && \
# INSTALL_PDO_MYSQL
    docker-php-ext-install pdo_mysql && \
# INSTALL_MYSQLI
    docker-php-ext-install mysqli && \
# INSTALL_MBSTRING
    docker-php-ext-install mbstring && \
# INSTALL_EXIF
    # Enable Exif if PHP extentions requirements
    docker-php-ext-install exif && \
# INSTALL_BCMATH
    # Install the bcmath extension
    docker-php-ext-install bcmath && \
# INSTALL_CALENDAR
    docker-php-ext-configure calendar && \
    docker-php-ext-install calendar && \
# INSTALL_SOCKETS
    docker-php-ext-install sockets && \
# INSTALL_GETTEXT
    apk add --no-cache gettext-dev && \
    docker-php-ext-install gettext && \
# INSTALL_SHMOP
    docker-php-ext-install shmop && \
# INSTALL_SYSVMSG
    docker-php-ext-install sysvmsg && \
# INSTALL_SYSVSEM
    docker-php-ext-install sysvsem && \
# INSTALL_SYSVSHM
    docker-php-ext-install sysvshm && \
# INSTALL_OPCACHE
    docker-php-ext-install opcache && \
# # INSTALL_PDO_FIREBIRD
#     docker-php-ext-install pdo_firebird && \
# # INSTALL_PDO_DBLIB
#     docker-php-ext-install pdo_dblib && \
# # INSTALL_PDO_OCI
#     docker-php-ext-install pdo_oci && \
# # INSTALL_PDO_ODBC
#     docker-php-ext-install pdo_odbc && \
# # INSTALL_PDO_PGSQL
#     apk --no-cache add postgresql-dev && \
#     docker-php-ext-install pdo_pgsql && \
# # INSTALL_PGSQL
#     # Install the pgsql extension
#     apk --no-cache add postgresql-dev && \
#     docker-php-ext-install pgsql && \
# # INSTALL_OCI8
#     docker-php-ext-install oci8 && \
# # INSTALL_ODBC
#     docker-php-ext-install odbc && \
# # INSTALL_DBA
#     docker-php-ext-install dba && \
# # INSTALL_INTERBASE
#     echo "Alpine linux do not support interbase/firebird!!!"
# # INSTALL_CURL
#     docker-php-ext-install curl && \
# # INSTALL_READLINE
# 	  apk add --no-cache readline-dev
# 	  apk add --no-cache libedit-dev
#     docker-php-ext-install readline && \
# # INSTALL_SNMP
#     apk add --no-cache net-snmp-dev && \
#     docker-php-ext-install snmp && \
# # INSTALL_PSPELL
#     apk add --no-cache aspell-dev && \
#     apk add --no-cache aspell-en && \
#     docker-php-ext-install pspell && \
# # INSTALL_RECODE
#     apk add --no-cache recode-dev && \
#     docker-php-ext-install recode && \
# # INSTALL_TIDY
#     apk add --no-cache 'tidyhtml-dev==5.2.0-r1' && \
#     docker-php-ext-install tidy && \
# # INSTALL_GMP
#     # Install the GMP extension
#     apk add --no-cache gmp-dev && \
#     docker-php-ext-install gmp && \
# # INSTALL_POSTGRESQL_CLIENT
#     apk add --no-cache postgresql-client && \
# # INSTALL_MYSQL_CLIENT
#     apk add --no-cache mysql-client && \
# # INSTALL_IMAP
# 	  apk add --no-cache imap-dev && \
#     docker-php-ext-configure imap --with-imap --with-imap-ssl && \
#     docker-php-ext-install imap && \
# # INSTALL_LDAP
# 	  apk add --no-cache ldb-dev && \
# 	  apk add --no-cache openldap-dev && \
#     docker-php-ext-install ldap && \ 
# INSTALL_IMAGICK
    apk add --no-cache file-dev imagemagick-dev && \
    printf "\n" | pecl install imagick && \
    docker-php-ext-enable imagick
# INSTALL_COMPOSER
ARG COMPOSER_REPO_PACKAGIST=https://mirrors.aliyun.com/composer/
    # Install composer and add its bin to the PATH.
RUN curl -s http://getcomposer.org/installer | php && \
    echo 'export PATH="$PATH:$HOME/.composer/vendor/bin"' >> ~/.bashrc && \
    mv composer.phar /usr/local/bin/composer  && \
    . ~/.bashrc &&\
# COMPOSER_REPO_PACKAGIST
    composer config -g repo.packagist composer ${COMPOSER_REPO_PACKAGIST} 
# INSTALL_REDIS
    # Install Php Redis Extension
RUN apk add --no-cache --update --virtual .phpize-deps $PHPIZE_DEPS && \
    printf "\n" | pecl install -o -f redis  && \
    docker-php-ext-enable redis && \
    rm -rf /usr/share/php && \
    rm -rf /tmp/*  && \
    apk del .phpize-deps 
# INSTALL_XDEBUG
#  #Install the xdebug extension
RUN if [ $(php -r "echo PHP_MAJOR_VERSION;") = "5" ]; then \
      pecl install xdebug-2.5.5; \
    else \
      pecl install xdebug; \
    fi && \
    docker-php-ext-enable xdebug 
# INSTALL_MEMCACHED
    # Install the php memcached extension
RUN apk add --no-cache libmemcached-dev zlib-dev && \
    if [ $(php -r "echo PHP_MAJOR_VERSION;") = "5" ]; then \
      pecl install memcached-2.2.0;  \
    else \
      pecl install memcached; \
    fi && \
      docker-php-ext-enable memcached
# # Install swoole extension
# # swoole require PHP version 5.5 or later.
#     # Install Php Swoole Extension
# 
# RUN if [ $(php -r "echo PHP_MAJOR_VERSION;") = "5" ]; then \
#       pecl install swoole-2.0.11; \
#     else \
#       if [ $(php -r "echo PHP_MINOR_VERSION;") = "0" ]; then \
#         pecl install swoole-2.2.0; \
#       else \
#         pecl install swoole; \
#       fi \
#     fi && \
#     docker-php-ext-enable swoole
# COMPOSER_PHPCSFIXER
RUN composer global require friendsofphp/php-cs-fixer && \
# COMPOSER_CODESNIFFER
    composer global require "squizlabs/php_codesniffer=*" && \
    composer clear-cache
# INSTALL_YAR
RUN pecl install yar && \
    docker-php-ext-enable yar
# # Check PHP version:
RUN php -v | head -n 1 | grep -q "PHP ${LARADOCK_PHP_VERSION}."
# Clean up
RUN apk del autoconf g++ make && rm -rf /tmp/* /var/cache/apk/*

RUN usermod -u 1000 www-data
