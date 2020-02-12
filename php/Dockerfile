FROM php:fpm

RUN apt-get update \
 && apt-get install -y --no-install-recommends \
    curl \
    libmemcached-dev \
    libz-dev \
    libpq-dev \
    libjpeg-dev \
    libpng-dev \
    libfreetype6-dev \
    libssl-dev \
    libmcrypt-dev \
    libzip-dev \
 && rm -rf /var/lib/apt/lists/*

RUN docker-php-ext-install pdo_mysql \
 && docker-php-ext-install pdo_pgsql \
 && docker-php-ext-install bcmath \
 && docker-php-ext-install zip \
 && docker-php-ext-configure gd \
 && docker-php-ext-install gd

RUN printf "\n" | pecl install redis \
 && docker-php-ext-enable redis
