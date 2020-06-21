FROM ubuntu

ENV DEBIAN_FRONTEND=noninteractive

USER root

ARG PUID=1000
ENV PUID ${PUID}
ARG PGID=1000
ENV PGID ${PGID}

RUN groupadd -g ${PGID} developer \
    && useradd -u ${PUID} -g developer -m developer

# UPGRADE BASE SYSTEM ############################################################################

RUN apt update \
    && apt upgrade -y

# ADDING REPO'S ##################################################################################

RUN apt install -y software-properties-common curl \
    && add-apt-repository -y ppa:ondrej/php \
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
    php-xdebug \
    wget \
    unzip \
    git \
    postgresql-client \
    redis-tools \
    nodejs \
    nano \
    zsh \
    dumb-init \
    && apt-get clean

RUN curl -s http://getcomposer.org/installer | php \
    && mv composer.phar /usr/local/bin/composer

RUN npm i -g yarn

# SETUP SHELL ####################################################################################

USER developer

RUN sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh) --keep-zshrc" \
    && echo '\n \
    alias art="php artisan"\n \
    alias pa="php artisan"\n \
    alias flush_redis="redis-cli -h redis flushall"' \
    >> /home/developer/.zshrc

# SETTING WORKDIR & RUN CMD ######################################################################

USER developer

ARG PATH_PROJECT_CONTAINER

WORKDIR ${PATH_PROJECT_CONTAINER}

ENTRYPOINT ["dumb-init", "--"]

CMD ["tail", "-f", "/dev/null"]
