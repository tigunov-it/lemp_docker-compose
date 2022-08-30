FROM php:8.1-fpm

ARG timezone

RUN apt-get update && apt-get install --no-install-recommends --no-install-suggests -y \
		git \
        curl \
        htop \
        libmemcached-dev \
        cron\
        unzip \
        nano \
        libicu-dev \
        zlib1g-dev \
        libssl-dev \
        pkg-config \
        libzip-dev \
        libpq-dev \
        librabbitmq-dev \
        libssh-dev \
    && rm -rf /var/lib/apt/lists/*

RUN set -xe \
    && docker-php-ext-configure \
        intl \
    && docker-php-ext-install \
        intl \
        opcache \
        zip \
        pdo \
        pdo_mysql \
        bcmath \
        sockets \
    && pecl install \
        apcu \
        xdebug \
        redis \
        memcached \
        amqp \
        && rm -rf /tmp/pear \
    && docker-php-ext-enable \
        apcu \
        xdebug \
        redis \
        memcached \
        amqp

COPY php.ini /usr/local/etc/php/php.ini
RUN sed -i -e "s#TIMEZONE#$timezone#g" /usr/local/etc/php/php.ini

COPY xdebug.ini /tmp/xdebug.ini
RUN cat /tmp/xdebug.ini >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN rm /tmp/xdebug.ini

COPY --from=composer:2.3.5 /usr/bin/composer /usr/bin/composer
ENV COMPOSER_ALLOW_SUPERUSER 1

RUN mkdir -p /var/www/.composer && chown -R www-data /var/www/.composer
WORKDIR /gb_hiload

COPY entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]
CMD ["php-fpm"]