FROM phusion/baseimage:master

ENV DEBIAN_FRONTEND=noninteractive

##################################################################################################
# UPGRADE BASE SYSTEM ############################################################################
##################################################################################################

RUN apt update \
 && apt upgrade -y \
 && apt-get install -y software-properties-common apt-utils curl

##################################################################################################
# ADDING REPO'S ##################################################################################
##################################################################################################

RUN add-apt-repository -y ppa:ondrej/php \
 && curl -sL https://deb.nodesource.com/setup_11.x | bash \
 && apt update \
 && apt upgrade -y

##################################################################################################
# INSTALL TOOLS ##################################################################################
##################################################################################################

RUN apt update \
 && apt install -y --allow-downgrades --allow-remove-essential --allow-change-held-packages \
    php7.3-cli \
    php7.3-common \
    php7.3-curl \
    php7.3-intl \
    php7.3-json \
    php7.3-xml \
    php7.3-mbstring \
    php7.3-mysql \
    php7.3-pgsql \
    php7.3-sqlite \
    php7.3-sqlite3 \
    php7.3-zip \
    php7.3-bcmath \
    php7.3-memcached \
    php7.3-gd \
    php7.3-dev \
    pkg-config \
    wget \
    unzip \
    libcurl4-openssl-dev \
    libedit-dev \
    libssl-dev \
    libxml2-dev \
    xz-utils \
    libsqlite3-dev \
    sqlite3 \
    git \
    nano \
    postgresql-client \
    redis-tools \
    nodejs \
 && apt-get clean

RUN npm install -g yarn

RUN curl -s http://getcomposer.org/installer | php \
 && echo "export PATH=$PATH:/var/www/vendor/bin" >> ~/.bashrc \
 && mv composer.phar /usr/local/bin/composer

##################################################################################################
# SETTING ALIASES ################################################################################
##################################################################################################

USER root

COPY ./aliases.sh /root/aliases.sh

RUN sed -i 's/\r//' /root/aliases.sh \
 && echo "" >> ~/.bashrc \
 && echo "# Load Custom Aliases" >> ~/.bashrc \
 && echo "source ~/aliases.sh" >> ~/.bashrc \
 && echo "" >> ~/.bashrc

##################################################################################################
# SETTING WORKDIR ################################################################################
##################################################################################################

ARG PATH_PROJECT_CONTAINER

WORKDIR ${PATH_PROJECT_CONTAINER}