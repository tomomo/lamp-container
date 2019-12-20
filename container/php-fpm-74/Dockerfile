FROM php:7.4-fpm-alpine

LABEL maintainer "tomomo <eclairpark@gmail.com>"

ENV PHPIZE_DEPS autoconf gcc g++ libtool make libc-dev libtool re2c

RUN apk add --no-cache \
  zip \
  bzip2-dev \
  gettext-dev \
  openldap-dev \
  aspell-dev \
  tidyhtml-dev \
  libxslt-dev \
  enchant-dev \
  gmp-dev \
  net-snmp-dev \
  icu-dev \
  libzip-dev \
  postgresql-dev \
  freetype-dev libpng-dev libjpeg-turbo-dev \
  krb5-dev imap-dev \
  imagemagick-dev \
  libedit-dev \
  curl-dev \
  recode-dev \
  ssmtp \
  oniguruma-dev \
  libgcrypt-dev \
  libgpg-error-dev \
  libffi-dev \
 && \
 apk add --no-cache --virtual .php-builddeps $PHPIZE_DEPS tzdata && \
 # timezone
 cp /usr/share/zoneinfo/Asia/Tokyo /etc/localtime && \
 # php
 pecl install imagick && \
 pecl install redis && \
 pecl install xdebug-2.9.0 && \
 docker-php-ext-enable imagick redis xdebug && \
 docker-php-ext-configure gd --with-freetype --with-jpeg && \
 NPROC=$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1) && \
 docker-php-ext-install -j${NPROC} \
  imap \
  bcmath \
  bz2 \
  calendar \
  ctype \
  curl \
  dba \
  enchant \
  exif \
  ftp \
  gd \
  gettext \
  gmp \
  intl \
  ldap \
  mbstring \
  mysqli pdo_mysql \
  pgsql pdo_pgsql \
  opcache \
  posix \
  pspell \
  readline \
  shmop \
  snmp \
  soap \
  sockets \
  sysvmsg \
  sysvsem \
  sysvshm \
  tidy \
  xmlrpc \
  xsl \
  zip \
  ffi \
 && \
 apk del .php-builddeps && \
 rm -rf /tmp/* /var/cache/apk/*

RUN cp -p /usr/local/etc/php/php.ini-development /usr/local/etc/php/php.ini && \
 sed -i -E 's/^(;| )*upload_max_filesize( )*=.*$/upload_max_filesize = 8M/g' /usr/local/etc/php/php.ini && \
 sed -i -E 's/^(;| )*date\.timezone( )*=.*$/date.timezone = Asia\/Tokyo/g' /usr/local/etc/php/php.ini && \
 sed -i -E 's/^(;| )*ffi\.enable( )*=.*$/ffi.enable = true/g' /usr/local/etc/php/php.ini && \
 { \
  echo '[mail function]'; \
  echo 'sendmail_path = "/usr/sbin/sendmail -t"'; \
 } >> /usr/local/etc/php/conf.d/mail.ini

RUN curl -sS https://getcomposer.org/installer | php && \
 mv composer.phar /usr/local/bin/composer

EXPOSE 9000
