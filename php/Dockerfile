FROM php:7.0-fpm

RUN cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && echo "Asia/Shanghai" > /etc/timezone

RUN mv /etc/apt/sources.list /etc/apt/sources.list.bak && \
    echo "deb http://mirrors.163.com/debian/ stretch main non-free contrib" >/etc/apt/sources.list && \
    echo "deb http://mirrors.163.com/debian/ stretch-updates main non-free contrib" >>/etc/apt/sources.list && \
    echo "deb http://mirrors.163.com/debian/ stretch-backports main non-free contrib" >>/etc/apt/sources.list && \
    echo "deb-src http://mirrors.163.com/debian/ stretch main non-free contrib" >>/etc/apt/sources.list && \
    echo "deb-src http://mirrors.163.com/debian/ stretch-updates main non-free contrib" >>/etc/apt/sources.list && \
    echo "deb-src http://mirrors.163.com/debian/ stretch-backports main non-free contrib" >>/etc/apt/sources.list && \
    echo "deb http://mirrors.163.com/debian-security/ stretch/updates main non-free contrib" >>/etc/apt/sources.list && \
    echo "deb-src http://mirrors.163.com/debian-security/ stretch/updates main non-free contrib" >>/etc/apt/sources.list


RUN apt-get update && apt-get upgrade -y && \
   apt-get install -y --no-install-recommends \
   git \
   libmemcached-dev \
   libz-dev \
   libpq-dev \
   libjpeg-dev \
   libfreetype6-dev \
   libssl-dev \
   libmcrypt-dev \
   libjpeg62-turbo-dev \
   libpng-dev \
   libicu-dev

RUN apt-get install -y libxml2-dev libbz2-dev re2c\
   libjpeg-dev libvpx-dev zlib1g-dev libgd-dev \
   libtidy-dev libxslt1-dev libmagic-dev libexif-dev file \
   sqlite3 libsqlite3-dev libxslt-dev

RUN docker-php-ext-install -j$(nproc) \
        iconv mcrypt gettext  mysqli pdo pdo_mysql zip bz2 exif pcntl\
        mbstring bcmath opcache xml simplexml sockets hash soap \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd

# install the soap extension
#RUN apt-get update -yqq && \
#   apt-get -y install libxml2-dev && \
#   docker-php-ext-install soap

# install Php Redis Extension
#RUN pecl install -o -f redis \
#   &&  rm -rf /tmp/pear \
#   &&  docker-php-ext-enable redis

# install the mongodb extension
#RUN pecl install mongodb && docker-php-ext-enable mongodb

# install intl and requirements
#RUN apt-get update -yqq && \
#   apt-get install -y g++ && \
#   docker-php-ext-configure intl && \
#   docker-php-ext-install intl

## install ds    extension
#RUN pecl install ds && \
#   docker-php-ext-enable ds

## install the ghostscript extension for PDF editing
#RUN apt-get update -yqq \
#   && apt-get install -y \
#   poppler-utils \
#   ghostscript

## imagick
#RUN apt-get update && apt-get install -y libmagickwand-6.q16-dev --no-install-recommends && \
#   ln -s /usr/lib/x86_64-linux-gnu/ImageMagick-6.8.9/bin-Q16/MagickWand-config /usr/bin && \
#   pecl install imagick && \
#   echo "extension=imagick.so" > /usr/local/etc/php/conf.d/ext-imagick.ini
##
## ssh2 module
#RUN apt-get install -y libssh2-1-dev && \
#   pecl install ssh2-1.0 && \
#   docker-php-ext-enable ssh2
#

## install the php memcache & memcached extension
#RUN set -x \
#   && apt-get update && apt-get install -y --no-install-recommends unzip libssl-dev libpcre3 libpcre3-dev \
#   && cd /tmp \
#   && curl -sSL -o php7.zip https://github.com/websupport-sk/pecl-memcache/archive/php7.zip \
#   && unzip php7 \
#   && cd pecl-memcache-php7 \
#   && /usr/local/bin/phpize \
#   && ./configure --with-php-config=/usr/local/bin/php-config \
#   && make \
#   && make install \
#   && echo "extension=memcache.so" > /usr/local/etc/php/conf.d/ext-memcache.ini \
#   && rm -rf /tmp/pecl-memcache-php7 php7.zip
#
#RUN curl -L -o /tmp/memcached.tar.gz "https://github.com/php-memcached-dev/php-memcached/archive/php7.tar.gz" \
#   && mkdir -p memcached \
#   && tar -C memcached -zxvf /tmp/memcached.tar.gz --strip 1 \
#   && ( \
#   cd memcached \
#   && phpize \
#   && ./configure \
#   && make -j$(nproc) \
#   && make install \
#   ) \
#   && rm -r memcached \
#   && rm /tmp/memcached.tar.gz \
#   && docker-php-ext-enable memcached

## mhash
#RUN apt-get install -y libmhash2 libmhash-dev libc-client-dev libkrb5-dev
#
#RUN docker-php-ext-configure hash --with-mhash && \
#   docker-php-ext-install hash

##imap
#RUN docker-php-ext-configure imap --with-kerberos --with-imap-ssl \
#   && docker-php-ext-install imap
#
## Opcache
#RUN docker-php-ext-install opcache

## install APCu
#RUN pecl install apcu-5.1.8 && \
#   pecl install apcu_bc-1.0.3 && \
#   docker-php-ext-enable apcu --ini-name 10-docker-php-ext-apcu.ini && \
#   docker-php-ext-enable apc --ini-name 20-docker-php-ext-apc.ini


## install the PHP gd library
#RUN docker-php-ext-configure gd \
#   --enable-gd-native-ttf \
#   --with-jpeg-dir=/usr/lib \
#   --with-freetype-dir=/usr/include/freetype2 && \
#   docker-php-ext-install gd


RUN apt-get update -y && apt-get install -y gnupg apt-transport-https locales
#set locale to utf-8
RUN echo "en_US.UTF-8 UTF-8" > /etc/locale.gen && \
   locale-gen
ENV LANG='en_US.UTF-8' LANGUAGE='en_US:en' LC_ALL='en_US.UTF-8'

#composer
RUN apt-get install -y wget && wget https://github.com/composer/composer/releases/download/1.8.6/composer.phar
RUN chmod u+x composer.phar
RUN mv composer.phar /usr/local/bin/composer
RUN composer config -g repo.packagist composer https://mirrors.aliyun.com/composer
CMD ["php-fpm", "-F"]
