FROM php:7.3-fpm-alpine
#FROM php:7.4.0RC1-fpm-alpine

LABEL maintainer="Lex Pal <uxnemka@gmail.com>"

RUN apk add --update --no-cache \
    bash \
    curl \
    zip \
    git \
    icu-dev \
    ; \
    docker-php-ext-install \
        intl \
    ; \
    docker-php-ext-enable \
		opcache \
    ;

COPY php.ini $PHP_INI_DIR/php.ini
COPY php.ini $PHP_INI_DIR/conf.d/php.ini

# Install XDebug
RUN apk add --no-cache $PHPIZE_DEPS \
    && pecl install xdebug \
    && docker-php-ext-enable xdebug

COPY xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

# Install composer
RUN EXPECTED_SIGNATURE=$(wget -q -O - https://composer.github.io/installer.sig) \
    && php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php -r "if (hash_file('SHA384', 'composer-setup.php') === '${EXPECTED_SIGNATURE}') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
    && php composer-setup.php --install-dir=/bin --filename=composer \
    && php -r "unlink('composer-setup.php');" \
    && chmod +x /bin/composer

# Install symfony CLI
RUN wget https://get.symfony.com/cli/installer -O - | bash \
    && mv /root/.symfony/bin/symfony /usr/local/bin/symfony

#CMD ["php-fpm", "-F"]

WORKDIR /var/www/html