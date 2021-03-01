ARG PHP_V

FROM php:${PHP_V}-fpm-alpine as base

RUN apk --update --no-cache add \
        git \
        openssh-client \
        curl \
        icu \
        icu-dev \
        libjpeg-turbo \
        libpng \
        freetype \
        libxslt \
        libxml2 \
        shadow \
        zlib \
        libzip \
    && apk --no-cache --virtual .deps add \
        g++ \
        make \
        libjpeg-turbo-dev \
        freetype-dev \
        libpng-dev \
        libxml2-dev \
        libxslt-dev \
        zlib-dev \
        libzip-dev \
        $PHPIZE_DEPS\
    && docker-php-ext-configure \
        gd --with-jpeg=/usr/local/ --with-freetype=/usr/include/ \
    && docker-php-ext-install \
        gd \
        bcmath \
        intl \
        pdo_mysql \
        soap \
        xsl \
        zip \
        sockets \
    && apk del .deps \
    && rm -rf /var/cache/apk/*

FROM base as composer

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

FROM composer as local

ARG USER=${USER}
ENV USER ${USER}

RUN apk --update --no-cache --virtual .deps add autoconf g++ make \
    && pecl install -f xdebug-2.9.8 \
    && docker-php-ext-enable xdebug \
    && apk del .deps \
    && apk --no-cache add bash nodejs npm \
    && rm -rf /var/cache/apk/* \
    && npm install -g grunt

RUN useradd -ms /bin/sh -U "$USER" \
    && usermod -a -G www-data "$USER"
