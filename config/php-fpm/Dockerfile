FROM php:7.3-fpm-alpine
#FROM php:7.4.0RC1-fpm-alpine

RUN apk add --update --no-cache \
    curl

# Install XDebug
RUN apk add --no-cache $PHPIZE_DEPS \
    && pecl install xdebug \
    && docker-php-ext-enable xdebug

COPY xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

#CMD ["/bin/sh"]