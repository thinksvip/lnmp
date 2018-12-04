ARG PHP_FPM_VERSION=7.2

FROM php:${PHP_FPM_VERSION}-fpm

ARG	TIME_ZONE

# Set Timezone

RUN ln -snf /usr/share/zoneinfo/"${TIME_ZONE}" /etc/localtime && echo "${TIME_ZONE}" > /etc/timezone

#COPY ./sources.list.stretch /etc/apt/sources.list

ARG REDIS_VERSION
ARG XDEBUG_VERSION
ARG IMAGICK_VERSION
ARG MEMCACHED_VERSION
ARG OPEN_UNIXSOCKE


# Extensions: ctype, dom, fileinfo, ftp, hash, iconv, json, pdo, pdo_sqlite, session,
# tokenizer, simplexml, xml, xmlreader, xmlwriter and phar are bundled and compiled into
# PHP by default. If missing, install them directly by `docker-php-ext-install extension_name`

# Notice:
# 1. Mcrypt was DEPRECATED in PHP 7.1.0, and REMOVED in PHP 7.2.0.
# 2. opcache requires PHP version >= 7.0.0.
# 3. soap requires libxml2-dev.
# 4. xml, xmlrpc, wddx require libxml2-dev and libxslt-dev.
# 5. Line `&& :\` is just for better reading and do nothing.
RUN apt-get update \
    && apt-get install -y libfreetype6-dev libjpeg62-turbo-dev libpng-dev  libxslt-dev \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install gd 

ARG INSTALL_INTL
# INSTALL_INTL
RUN if [ ${INSTALL_INTL} = true ]; then \
    # Install intl and requirements
    apt-get install -y zlib1g-dev libicu-dev g++ && \
    docker-php-ext-configure intl && \
    docker-php-ext-install intl \
;fi
ARG INSTALL_SOAP
# INSTALL_SOAP
RUN if [ ${INSTALL_SOAP} = true ]; then \
    # Install the soap extension
    rm /etc/apt/preferences.d/no-debian-php && \
    apt-get -y install libxml2-dev php-soap && \
    docker-php-ext-install soap \
;fi
ARG INSTALL_XSL
# INSTALL_XSL
RUN if [ ${INSTALL_XSL} = true ]; then \
    docker-php-ext-install xsl \
;fi
ARG INSTALL_XMLRPC
# INSTALL_XMLRPC
RUN if [ ${INSTALL_XMLRPC} = true ]; then \
    docker-php-ext-install xmlrpc \
;fi
ARG INSTALL_WDDX
# INSTALL_WDDX
RUN if [ ${INSTALL_WDDX} = true ]; then \
    docker-php-ext-install wddx \
;fi
ARG INSTALL_BZ2
# INSTALL_BZ2
RUN if [ ${INSTALL_BZ2} = true ]; then \
    apt-get install -y libbz2-dev && \
    docker-php-ext-install bz2 \
;fi
ARG INSTALL_ZIP
# INSTALL_ZIP
RUN if [ ${INSTALL_ZIP} = true ]; then \
    apt-get install libzip-dev -y && \
    docker-php-ext-configure zip --with-libzip && \
    # Install the zip extension
    docker-php-ext-install zip \
;fi
ARG INSTALL_PCNTL
# INSTALL_PCNTL
RUN if [ ${INSTALL_PCNTL} = true ]; then \
    # Installs pcntl, helpful for running Horizon
    docker-php-ext-install pcntl \
;fi
ARG INSTALL_PDO_MYSQL
# INSTALL_PDO_MYSQL
RUN if [ ${INSTALL_PDO_MYSQL} = true ]; then \
    docker-php-ext-install pdo_mysql \
;fi
ARG INSTALL_MYSQLI
# INSTALL_MYSQLI
RUN if [ ${INSTALL_MYSQLI} = true ]; then \
    docker-php-ext-install mysqli \
;fi
ARG INSTALL_MBSTRING
# INSTALL_MBSTRING
RUN if [ ${INSTALL_MBSTRING} = true ]; then \
    docker-php-ext-install pdo_mysql \
;fi
ARG INSTALL_EXIF
# INSTALL_EXIF
RUN if [ ${INSTALL_EXIF} = true ]; then \
    # Enable Exif if PHP extentions requirements
    docker-php-ext-install exif \
;fi
ARG INSTALL_BCMATH
# INSTALL_BCMATH
RUN if [ ${INSTALL_BCMATH} = true ]; then \
    # Install the bcmath extension
    docker-php-ext-install bcmath \
;fi
USER root
ARG INSTALL_CALENDAR
# INSTALL_CALENDAR
RUN if [ ${INSTALL_CALENDAR} = true ]; then \
    docker-php-ext-configure calendar && \
    docker-php-ext-install calendar \
;fi
ARG INSTALL_SOCKETS
# INSTALL_SOCKETS
RUN if [ ${INSTALL_SOCKETS} = true ]; then \
    docker-php-ext-install sockets \
;fi
ARG INSTALL_GETTEXT
# INSTALL_GETTEXT
RUN if [ ${INSTALL_GETTEXT} = true ]; then \
    docker-php-ext-install gettext \
;fi
ARG INSTALL_SHMOP
# INSTALL_SHMOP
RUN if [ ${INSTALL_SHMOP} = true ]; then \
    docker-php-ext-install shmop \
;fi
ARG INSTALL_SYSVMSG
# INSTALL_SYSVMSG
RUN if [ ${INSTALL_SYSVMSG} = true ]; then \
    docker-php-ext-install sysvmsg \
;fi
ARG INSTALL_SYSVSEM
# INSTALL_SYSVSEM
RUN if [ ${INSTALL_SYSVSEM} = true ]; then \
    docker-php-ext-install sysvsem \
;fi
ARG INSTALL_SYSVSHM
# INSTALL_SYSVSHM
RUN if [ ${INSTALL_SYSVSHM} = true ]; then \
    docker-php-ext-install sysvshm \
;fi
ARG INSTALL_OPCACHE
# INSTALL_OPCACHE
RUN if [ ${INSTALL_OPCACHE} = true ]; then \
    docker-php-ext-install opcache \
;fi
ARG INSTALL_PDO_FIREBIRD
# INSTALL_PDO_FIREBIRD
RUN if [ ${INSTALL_PDO_FIREBIRD} = true ]; then \
    docker-php-ext-install pdo_firebird \
;fi
ARG INSTALL_PDO_DBLIB
# INSTALL_PDO_DBLIB
RUN if [ ${INSTALL_PDO_DBLIB} = true ]; then \
    docker-php-ext-install pdo_dblib \
;fi
ARG INSTALL_PDO_OCI
# INSTALL_PDO_OCI
RUN if [ ${INSTALL_PDO_OCI} = true ]; then \
    docker-php-ext-install pdo_oci \
;fi
ARG INSTALL_PDO_ODBC
# INSTALL_PDO_ODBC
RUN if [ ${INSTALL_PDO_ODBC} = true ]; then \
    docker-php-ext-install pdo_odbc \
;fi
ARG INSTALL_PDO_PGSQL
# INSTALL_PDO_PGSQL
RUN if [ ${INSTALL_PDO_PGSQL} = true ]; then \
    docker-php-ext-install pdo_pgsql \
;fi
ARG INSTALL_PGSQL
# INSTALL_PGSQL
RUN if [ ${INSTALL_PGSQL} = true ]; then \
    # Install the pgsql extension
    docker-php-ext-install pgsql \
;fi
ARG INSTALL_PG_CLIENT
RUN if [ ${INSTALL_PG_CLIENT} = true ]; then \
    # Create folders if not exists (https://github.com/tianon/docker-brew-debian/issues/65)
    mkdir -p /usr/share/man/man1 && \
    mkdir -p /usr/share/man/man7 && \
    # Install the pgsql client
    apt-get install -y postgresql-client \
;fi
ARG INSTALL_OCI8
# INSTALL_OCI8
RUN if [ ${INSTALL_OCI8} = true ]; then \
    docker-php-ext-install oci8 \
;fi
ARG INSTALL_ODBC
# INSTALL_ODBC
RUN if [ ${INSTALL_ODBC} = true ]; then \
    docker-php-ext-install odbc \
;fi
ARG INSTALL_DBA
# INSTALL_DBA
RUN if [ ${INSTALL_DBA} = true ]; then \
    docker-php-ext-install dba \
;fi
ARG INSTALL_INTERBASE
# INSTALL_INTERBASE
RUN if [ ${INSTALL_INTERBASE} = true ]; then \
    docker-php-ext-install interbase \
;fi
ARG INSTALL_MCRYPT
# INSTALL_MCRYPT
RUN if [ ${INSTALL_MCRYPT} = true ]; then \
    apt-get install -y libmcrypt-dev && \
    docker-php-ext-install mcrypt \
;fi
ARG INSTALL_CURL
# INSTALL_CURL
RUN if [ ${INSTALL_CURL} = true ]; then \
    apt-get install -y curl && \
    apt-get install -y libcurl3 && \
    apt-get install -y libcurl4-openssl-dev && \
    docker-php-ext-install curl \
;fi
ARG INSTALL_READLINE
# INSTALL_READLINE
RUN if [ ${INSTALL_READLINE} = true ]; then \
    apt-get install -y libreadline-dev && \
    docker-php-ext-install readline \
;fi
ARG INSTALL_SNMP
# INSTALL_SNMP
RUN if [ ${INSTALL_SNMP} = true ]; then \
    apt-get install -y libsnmp-dev && \
    apt-get install -y snmp && \
    docker-php-ext-install snmp \
;fi
ARG INSTALL_PSPELL
# INSTALL_PSPELL
RUN if [ ${INSTALL_PSPELL} = true ]; then \
    apt-get install -y libpspell-dev && \
    apt-get install -y aspell-en && \
    docker-php-ext-install pspell \
;fi
ARG INSTALL_RECODE
# INSTALL_RECODE
RUN if [ ${INSTALL_RECODE} = true ]; then \
    apt-get install -y librecode0 && \
    apt-get install -y librecode-dev && \
    docker-php-ext-install recode \
;fi
ARG INSTALL_TIDY
# INSTALL_TIDY
RUN if [ ${INSTALL_TIDY} = true ]; then \
    apt-get install -y libtidy-dev && \
    docker-php-ext-install tidy \
;fi
ARG INSTALL_GMP
# INSTALL_GMP
RUN if [ ${INSTALL_GMP} = true ]; then \
    # Install the GMP extension
	  apt-get install -y libgmp-dev && \
      if [ $(php -r "echo PHP_MAJOR_VERSION;") = "5" ]; then \
      ln -s /usr/include/x86_64-linux-gnu/gmp.h /usr/include/gmp.h \
    ;fi && \
    docker-php-ext-install gmp \
;fi
ARG INSTALL_POSTGRESQL_CLIENT
# INSTALL_POSTGRESQL_CLIENT
RUN if [ ${INSTALL_POSTGRESQL_CLIENT} = true ]; then \
    apt-get install -y postgresql-client \
;fi
ARG INSTALL_MYSQL_CLIENT
# INSTALL_MYSQL_CLIENT
RUN if [ ${INSTALL_MYSQL_CLIENT} = true ]; then \
    apt-get install -y mysql-client \
;fi
ARG INSTALL_IMAP
# INSTALL_IMAP
RUN if [ ${INSTALL_IMAP} = true ]; then \
    apt-get install -y libc-client-dev libkrb5-dev && \
    rm -r /var/lib/apt/lists/* && \
    docker-php-ext-configure imap --with-kerberos --with-imap-ssl && \
    docker-php-ext-install imap \
;fi
ARG INSTALL_LDAP
# INSTALL_LDAP
RUN if [ ${INSTALL_LDAP} = true ]; then \
    apt-get install -y libldap2-dev && \
    docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/ && \
    docker-php-ext-install ldap \
;fi
ARG INSTALL_COMPOSER
# INSTALL_COMPOSER
USER root
RUN if [ ${INSTALL_COMPOSER} = true ]; then \
    # Install composer and add its bin to the PATH.
    curl -s http://getcomposer.org/installer | php && \
    echo 'export PATH="$PATH:$HOME/.composer/vendor/bin"' >> ~/.bashrc && \
    mv composer.phar /usr/local/bin/composer  && \
    . ~/.bashrc \
;fi
ARG COMPOSER_REPO_PACKAGIST
# COMPOSER_REPO_PACKAGIST
USER root
RUN if [ ${COMPOSER_REPO_PACKAGIST} ]; then \
    composer config -g repo.packagist composer ${COMPOSER_REPO_PACKAGIST} \
;fi
ARG INSTALL_REDIS
# INSTALL_REDIS
RUN if [ ${INSTALL_REDIS} = true ]; then \
    # Install Php Redis Extension
    printf "\n" | pecl install -o -f redis \
    &&  rm -rf /tmp/pear \
    &&  docker-php-ext-enable redis \
;fi
ARG INSTALL_XDEBUG
# INSTALL_XDEBUG
RUN if [ ${INSTALL_XDEBUG} = true ]; then \
  # Install the xdebug extension
  if [ $(php -r "echo PHP_MAJOR_VERSION;") = "5" ]; then \
    pecl install xdebug-2.5.5; \
  else \
    pecl install xdebug; \
  fi && \
    docker-php-ext-enable xdebug \
;fi
USER root
ARG INSTALL_IMAGICK
RUN if [ ${INSTALL_IMAGICK} = true ]; then \
    apt-get install -y libmagickwand-dev imagemagick && \
    pecl install imagick && \
    docker-php-ext-enable imagick \
;fi
ARG INSTALL_MEMCACHED
# INSTALL_MEMCACHED
RUN if [ ${INSTALL_MEMCACHED} = true ]; then \
    # Install the php memcached extension
    apt-get install -y libmemcached-dev zlib1g-dev && \
    if [ $(php -r "echo PHP_MAJOR_VERSION;") = "5" ]; then \
      pecl install memcached-2.2.0;  \
    else \
      pecl install memcached-3.0.4; \
    fi && \
      docker-php-ext-enable memcached \
;fi
ARG INSTALL_SWOOLE
# Install swoole extension
# swoole require PHP version 5.5 or later.
RUN if [ ${INSTALL_SWOOLE} = true ]; then \
    # Install Php Swoole Extension
    if [ $(php -r "echo PHP_MAJOR_VERSION;") = "5" ]; then \
      pecl install swoole-2.0.11; \
    else \
      if [ $(php -r "echo PHP_MINOR_VERSION;") = "0" ]; then \
        pecl install swoole-2.2.0; \
      else \
        pecl install swoole; \
      fi \
    fi && \
    docker-php-ext-enable swoole \
;fi
ARG COMPOSER_PHPCSFIXER
# COMPOSER_PHPCSFIXER
RUN if [ ${COMPOSER_PHPCSFIXER} = true ]; then \
   composer global require friendsofphp/php-cs-fixer \
;fi
ARG COMPOSER_CODESNIFFER
# COMPOSER_CODESNIFFER
RUN if [ ${COMPOSER_CODESNIFFER} = true ]; then \
   composer global require "squizlabs/php_codesniffer=*" \
;fi

# Check PHP version:
RUN php -v | head -n 1 | grep -q "PHP ${LARADOCK_PHP_VERSION}."

USER root

# Clean up
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    rm /var/log/lastlog /var/log/faillog