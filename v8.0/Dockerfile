FROM php:8.0.2-alpine

RUN apk update && apk add --no-cache $PHPIZE_DEPS \
    && apk add --no-cache zlib-dev gd-dev libwebp-dev freetype-dev libpng-dev libjpeg-turbo-dev \
        curl zip libzip-dev gmp-dev \
        #librdkafka-dev \
        imap-dev openssl-dev git zsh

RUN docker-php-ext-configure gd --enable-gd \
        --with-freetype \
        --with-jpeg \
        --with-webp \
    && docker-php-ext-install gd \
    && docker-php-ext-configure bcmath && docker-php-ext-install bcmath \
    && docker-php-ext-configure zip --with-zip && docker-php-ext-install zip \
    && docker-php-ext-install gmp \
    && PHP_OPENSSL=yes docker-php-ext-configure imap --with-imap-ssl \
    && docker-php-ext-install imap

ARG PHP_XDEBUG_VERSION=3.0.1
RUN pecl install xdebug-${PHP_XDEBUG_VERSION} && docker-php-ext-enable xdebug

#ARG PHP_RDKAFKA_VERSION=4.1.2
#RUN pecl install rdkafka-${PHP_RDKAFKA_VERSION} && docker-php-ext-enable rdkafka

# Install composer
ARG COMPOSER_VERSION=2.0.8
RUN curl -sS https://getcomposer.org/installer | php -- \
        --filename=composer \
        --version=${COMPOSER_VERSION} \
        --install-dir=/usr/local/bin
RUN mkdir -m 777 -p /var/composer/
ENV COMPOSER_HOME /var/composer/
