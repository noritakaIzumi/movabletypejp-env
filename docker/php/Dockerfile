################################################################################
# Copy files
################################################################################
FROM busybox:latest AS build

COPY xdebug.ini-override* /
# Skip if file exists
RUN cp -n xdebug.ini-override.template xdebug.ini-override

################################################################################
# PHP
################################################################################
FROM php:8.1.13-fpm-alpine3.17

RUN apk update \
    && apk add --update linux-headers \
    && apk add --no-cache $PHPIZE_DEPS \
    && pecl install xdebug-3.2.0 \
    && docker-php-ext-enable xdebug \
    && apk del --purge $PHPIZE_DEPS \
    && [ -d "/var/cache/apk" ] && rm -rf /var/cache/apk/* \
    || true

COPY --from=build xdebug.ini-override /tmp/xdebug.ini-override
RUN cat /tmp/xdebug.ini-override >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

WORKDIR /usr/local/apache2
