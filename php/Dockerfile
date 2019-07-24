ARG PHP_FPM_VERSION=7.2

FROM php:${PHP_FPM_VERSION}-fpm

ARG	TIME_ZONE=Asia/Shanghai

# Set Timezone

RUN ln -snf /usr/share/zoneinfo/"${TIME_ZONE}" /etc/localtime && echo "${TIME_ZONE}" > /etc/timezone

#COPY ./sources.list.stretch /etc/apt/sources.list

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
# INSTALL_INTL

    # Install intl and requirements
USER root
RUN apt-get install -y zlib1g-dev libicu-dev g++ && \
    docker-php-ext-configure intl && \
    docker-php-ext-install intl && \
    # Install the soap extension
    rm /etc/apt/preferences.d/no-debian-php && \
    apt-get -y install libxml2-dev php-soap && \
    docker-php-ext-install soap && \
# INSTALL_XSL
    docker-php-ext-install xsl && \
# INSTALL_XMLRPC
    docker-php-ext-install xmlrpc && \
# INSTALL_WDDX
    docker-php-ext-install wddx && \
# INSTALL_BZ2
    apt-get install -y libbz2-dev && \
    docker-php-ext-install bz2 && \
# INSTALL_ZIP
    apt-get install libzip-dev -y && \
    docker-php-ext-configure zip --with-libzip && \
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
    docker-php-ext-install pdo_mysql && \
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
#     docker-php-ext-install pdo_pgsql && \
# # INSTALL_PGSQL
#     # Install the pgsql extension
#     docker-php-ext-install pgsql && \
#     # Create folders if not exists (https://github.com/tianon/docker-brew-debian/issues/65)
#     mkdir -p /usr/share/man/man1 && \
#     mkdir -p /usr/share/man/man7 && \
#     # Install the pgsql client
#     apt-get install -y postgresql-client && \
# # INSTALL_OCI8
#     docker-php-ext-install oci8 && \
# # INSTALL_ODBC
#     docker-php-ext-install odbc && \
# # INSTALL_DBA
#     docker-php-ext-install dba && \
# # INSTALL_INTERBASE
#     docker-php-ext-install interbase && \
# # INSTALL_MCRYPT
#     apt-get install -y libmcrypt-dev && \
#     docker-php-ext-install mcrypt && \
# # INSTALL_CURL
#     apt-get install -y curl && \
#     apt-get install -y libcurl3 && \
#     apt-get install -y libcurl4-openssl-dev && \
#     docker-php-ext-install curl && \
# # INSTALL_READLINE
#     apt-get install -y libreadline-dev && \
#     docker-php-ext-install readline && \
# # INSTALL_SNMP
#     apt-get install -y libsnmp-dev && \
#     apt-get install -y snmp && \
#     docker-php-ext-install snmp && \
# # INSTALL_PSPELL
#     apt-get install -y libpspell-dev && \
#     apt-get install -y aspell-en && \
#     docker-php-ext-install pspell && \
# # INSTALL_RECODE
#     apt-get install -y librecode && \
#     apt-get install -y librecode-dev && \
#     docker-php-ext-install recode && \
# # INSTALL_TIDY
#     apt-get install -y libtidy-dev && \
#     docker-php-ext-install tidy && \
# INSTALL_GMP
    # Install the GMP extension
	apt-get install -y libgmp-dev && \
    if [ $(php -r "echo PHP_MAJOR_VERSION;") = "5" ]; then \
      ln -s /usr/include/x86_64-linux-gnu/gmp.h /usr/include/gmp.h \
    ;fi && \
    docker-php-ext-install gmp
# # INSTALL_POSTGRESQL_CLIENT
#     apt-get install -y postgresql-client && \
# # INSTALL_MYSQL_CLIENT
#     apt-get install -y mysql-client && \
# # INSTALL_IMAP
#     apt-get install -y libc-client-dev libkrb5-dev && \
#     rm -r /var/lib/apt/lists/* && \
#     docker-php-ext-configure imap --with-kerberos --with-imap-ssl && \
#     docker-php-ext-install imap && \
# # INSTALL_LDAP
#     apt-get install -y libldap2-dev && \
#     docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/ && \
#     docker-php-ext-install ldap && \
# INSTALL_COMPOSER
USER root
ARG COMPOSER_REPO_PACKAGIST=https://packagist.phpcomposer.com
    # Install composer and add its bin to the PATH.
RUN curl -s http://getcomposer.org/installer | php && \
    echo 'export PATH="$PATH:$HOME/.composer/vendor/bin"' >> ~/.bashrc && \
    mv composer.phar /usr/local/bin/composer  && \
    . ~/.bashrc &&\
# COMPOSER_REPO_PACKAGIST
    composer config -g repo.packagist composer ${COMPOSER_REPO_PACKAGIST} 
# INSTALL_REDIS
    # Install Php Redis Extension
USER root
RUN printf "\n" | pecl install -o -f redis \
    &&  rm -rf /tmp/pear \
    &&  docker-php-ext-enable redis 
# INSTALL_XDEBUG
  # Install the xdebug extension
# USER root
# RUN if [ $(php -r "echo PHP_MAJOR_VERSION;") = "5" ]; then \
#       pecl install xdebug-2.5.5; \
#     else \
#       pecl install xdebug; \
#     fi && \
#     docker-php-ext-enable xdebug 
USER root
RUN apt-get install -y libmagickwand-dev imagemagick && \
    pecl install imagick && \
    docker-php-ext-enable imagick
# INSTALL_MEMCACHED
    # Install the php memcached extension
RUN apt-get install -y libmemcached-dev zlib1g-dev && \
    if [ $(php -r "echo PHP_MAJOR_VERSION;") = "5" ]; then \
      pecl install memcached-2.2.0;  \
    else \
      pecl install memcached-3.0.4; \
    fi && \
      docker-php-ext-enable memcached
# # Install swoole extension
# # swoole require PHP version 5.5 or later.
#     # Install Php Swoole Extension
# USER root
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
# USER root
# RUN composer global require friendsofphp/php-cs-fixer && \
# # COMPOSER_CODESNIFFER
#    composer global require "squizlabs/php_codesniffer=*" 
# # Check PHP version:
RUN php -v | head -n 1 | grep -q "PHP ${LARADOCK_PHP_VERSION}."

USER root

# Clean up
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    rm /var/log/lastlog /var/log/faillog