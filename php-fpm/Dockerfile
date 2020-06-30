FROM php:fpm

ENV DEBIAN_FRONTEND=noninteractive

USER root

# SETUP BASE SYSTEM ###############################################################################

RUN apt update && \
    apt upgrade -y && \
    apt install -y apt-utils && \
    apt autoremove -y --purge && \
    docker-php-ext-install opcache

COPY ./opcache.ini /usr/local/etc/php/conf.d/opcache.ini

ARG PUID=1000
ENV PUID ${PUID}
ARG PGID=1000
ENV PGID ${PGID}
RUN groupmod -o -g ${PGID} www-data && \
    usermod -o -u ${PUID} -g www-data www-data

# SET TIMEZONE ####################################################################################

USER root

ARG TIMEZONE=UTC
ENV TZ ${TIMEZONE}

RUN ln -snf /usr/share/zoneinfo/${TZ} /etc/localtime && echo ${TZ} > /etc/timezone

# INSTALL GD ######################################################################################

ARG INSTALL_GD=false
RUN if [ ${INSTALL_GD} = true ]; then \
    apt install -y zlib1g-dev libpng-dev libjpeg-dev libfreetype6-dev && \
    docker-php-ext-configure gd --with-jpeg --with-freetype && \
    docker-php-ext-install gd \
    ;fi

# INSTALL ZIP #####################################################################################

ARG INSTALL_ZIP=false
RUN if [ ${INSTALL_ZIP} = true ]; then \
    apt install -y libzip-dev && \
    docker-php-ext-install zip \
    ;fi

# INSTALL BCMATH ##################################################################################

ARG INSTALL_BCMATH=false
RUN if [ ${INSTALL_BCMATH} = true ]; then \
    docker-php-ext-install bcmath \
    ;fi

# INSTALL PGSQL ###################################################################################

ARG INSTALL_PGSQL=false
RUN if [ ${INSTALL_PGSQL} = true ]; then \
    apt install -y libpq-dev && \
    docker-php-ext-install pdo_pgsql \
    ;fi

# INSTALL MYSQL ###################################################################################

ARG INSTALL_MYSQL=false
RUN if [ ${INSTALL_MYSQL} = true ]; then \
    docker-php-ext-install pdo_mysql \
    ;fi

# INSTALL MONGODB #################################################################################

ARG INSTALL_MONGODB=false
RUN if [ ${INSTALL_MONGODB} = true ]; then \
    pecl install mongodb && \
    docker-php-ext-enable mongodb \
    ;fi

# INSTALL REDIS ###################################################################################

ARG INSTALL_REDIS=false
RUN if [ ${INSTALL_REDIS} = true ]; then \
    pecl install redis && \
    docker-php-ext-enable redis \
    ;fi

# DISABLE OPCACHE #################################################################################

ARG ENABLE_OPCACHE=true
RUN if [ ${ENABLE_OPCACHE} = false ]; then \
    sed -i 's/opcache.enable=1/opcache.enable=0/g' /usr/local/etc/php/conf.d/opcache.ini \
    ;fi

# INSTALL XDEBUG ##################################################################################

ARG INSTALL_XDEBUG=false
RUN if [ ${INSTALL_XDEBUG} = true ]; then \
    pecl install xdebug && \
    docker-php-ext-enable xdebug \
    ;fi

# CLEAN UP ########################################################################################

RUN apt clean && \
    apt autoremove --purge && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
