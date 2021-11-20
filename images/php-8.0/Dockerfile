FROM php:8.0-cli

WORKDIR /var/www/html

ENV DEBIAN_FRONTEND noninteractive

COPY --from=mlocati/php-extension-installer /usr/bin/install-php-extensions /usr/bin/

RUN apt-get update \
    # install dependencies
    && apt-get install -y  zip unzip cron curl supervisor \
    # install php extensions
    && install-php-extensions bcmath bz2 exif gd imagick imap intl memcached opcache pcntl pdo_mysql pdo_pgsql sockets redis shmop soap zip \
    # remove default php.ini file
    && rm /usr/local/etc/php/php.ini-* \
    # install composer
    && curl -sS https://getcomposer.org/installer | php \
    && mv composer.phar /usr/local/bin/composer \
    # cleanup
    && apt-get -y autoremove \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* \
    # create entrypoints directory
    && mkdir -p /usr/local/entrypoints

COPY start-container /usr/local/bin/start-container
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf
COPY php.ini /usr/local/etc/php/conf.d/php.ini

RUN chmod +x /usr/local/bin/start-container

EXPOSE 80

ENTRYPOINT ["start-container"]