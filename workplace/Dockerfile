FROM ubuntu

ENV DEBIAN_FRONTEND=noninteractive

USER root

# UPGRADE BASE SYSTEM ############################################################################

RUN apt update \
 && apt upgrade -y

# ADDING REQUIRED PACKAGES #######################################################################

RUN apt install -y software-properties-common curl

# ADDING REPO'S ##################################################################################

RUN add-apt-repository -y ppa:ondrej/php \
 && add-apt-repository ppa:git-core/ppa \
 && curl -sL https://deb.nodesource.com/setup_14.x | bash \
 && apt update \
 && apt upgrade -y

# INSTALL TOOLS ##################################################################################

RUN apt update \
 && apt install -y --allow-downgrades --allow-remove-essential --allow-change-held-packages \
    php-cli \
    php-curl \
    php-json \
    php-xml \
    php-mbstring \
    php-redis \
    php-mysql \
    php-pgsql \
    php-zip \
    php-bcmath \
    php-memcached \
    php-gd \
    php-mongodb \
    wget \
    unzip \
    git \
    postgresql-client \
    redis-tools \
    nodejs \
    nano \
    zsh \
 && apt-get clean

RUN npm i -g yarn

RUN curl -s http://getcomposer.org/installer | php \
 && mv composer.phar /usr/local/bin/composer

RUN sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" "" --unattended

# SETTING WORKDIR & RUN CMD ######################################################################

ARG PATH_PROJECT_CONTAINER

WORKDIR ${PATH_PROJECT_CONTAINER}

COPY zshrc /root/.zshrc

ENTRYPOINT ["tail", "-f", "/dev/null"]