FROM php:7.3-fpm-alpine
#FROM php:7.4.0RC1-fpm-alpine

LABEL maintainer="Lex Pal <uxnemka@gmail.com>"

RUN apk add --update --no-cache \
    curl \
    zip

# Install XDebug
RUN apk add --no-cache $PHPIZE_DEPS \
    && pecl install xdebug \
    && docker-php-ext-enable xdebug

COPY xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

# Install composer
#RUN echo "$(curl -sS https://composer.github.io/installer.sig) -" > composer-setup.php.sig \
#    && curl -sS https://getcomposer.org/installer | tee composer-setup.php | sha384sum -c composer-setup.php.sig \
#    && php composer-setup.php && rm composer-setup.php* \
#    && chmod +x composer.phar && mv composer.phar /usr/bin/composer

RUN EXPECTED_SIGNATURE=$(wget -q -O - https://composer.github.io/installer.sig) \
    && php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php -r "if (hash_file('SHA384', 'composer-setup.php') === ${EXPECTED_SIGNATURE}) { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
    && php composer-setup.php --install-dir=/bin --filename=composer \
    && php -r "unlink('composer-setup.php');" \
    && chmod +x /bin/composer

#RUN curl -o /tmp/composer-setup.php https://getcomposer.org/installer \
#    && curl -o /tmp/composer-setup.sig https://composer.github.io/installer.sig \
#     Make sure we're installing what we think we're installing!
#    && php -r "if (hash('SHA384', file_get_contents('/tmp/composer-setup.php')) !== trim(file_get_contents('/tmp/composer-setup.sig'))) { unlink('/tmp/composer-setup.php'); echo 'Invalid installer' . PHP_EOL; exit(1); }" \
#    && php /tmp/composer-setup.php --no-ansi --install-dir=/usr/local/bin --filename=composer --snapshot \
#    && rm -f /tmp/composer-setup.*

# Install composer
#RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
#RUN php composer-setup.php
#RUN php -r "unlink('composer-setup.php');"
#RUN mv composer.phar /usr/local/bin/composer
#RUN mkdir -p /var/www/.composer
#COPY composer/config.json /var/www/.composer/config.json
#RUN chown -R ${USER_ID}:${GROUP_ID} /var/www/.composer && \
#    chown ${USER_ID}:${GROUP_ID} /var/www

#RUN curl -sS https://getcomposer.org/installer | php -- \
#    --install-dir=/usr/bin --filename=composer \
#    && chmod +x /usr/bin/composer

#CMD ["php-fpm7", "-F"]

WORKDIR /var/www/html