FROM ubuntu

ENV DEBIAN_FRONTEND=noninteractive
ENV TERM xterm

# UPGRADE BASE SYSTEM #############################################################################

USER root

RUN set -xe && \
    apt update && \
    apt upgrade -y

# ADDING REPO'S ###################################################################################

USER root

RUN apt install -y software-properties-common curl && \
    add-apt-repository -y ppa:ondrej/php && \
    add-apt-repository -y ppa:git-core/ppa && \
    curl -sL https://deb.nodesource.com/setup_14.x | bash && \
    apt update && \
    apt upgrade -y

# SET TIMEZONE ####################################################################################

USER root

ARG TIMEZONE=UTC
ENV TZ ${TIMEZONE}

RUN ln -snf /usr/share/zoneinfo/${TZ} /etc/localtime && echo ${TZ} > /etc/timezone

# INSTALL BASE ####################################################################################

USER root

RUN apt-get install -y --allow-downgrades --allow-remove-essential --allow-change-held-packages \
    php-cli \
    php-common \
    php-curl \
    php-json \
    php-xml \
    php-mbstring \
    php-dev \
    pkg-config \
    nodejs \
    unzip \
    git \
    curl \
    vim \
    nano \
    wget \
    dumb-init && \
    apt-get clean && \
    curl -s http://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer && \
    npm i -g yarn

# ADD DEVELOPER ###################################################################################

USER root

ARG PUID=1000
ENV PUID ${PUID}
ARG PGID=1000
ENV PGID ${PGID}

RUN groupadd -g ${PGID} developer && \
    useradd -u ${PUID} -g developer -m developer && \
    usermod -p "*" developer -s /bin/bash

# BASH ALIASES ####################################################################################

USER root

COPY ./aliases.sh /root/aliases.sh

RUN echo "" >> ~/.bashrc && \
    echo "source ~/aliases.sh" >> ~/.bashrc

USER developer

COPY ./aliases.sh /home/developer/aliases.sh

RUN echo "" >> ~/.bashrc && \
    echo "source ~/aliases.sh" >> ~/.bashrc

# OH MY ZSH #######################################################################################

USER root

ARG INSTALL_ZSH=false

RUN if [ ${INSTALL_ZSH} = true ]; then \
    apt install -y zsh \
    ;fi

USER developer

RUN if [ ${INSTALL_ZSH} = true ]; then \
    sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh) --keep-zshrc" && \
    echo "source ~/aliases.sh" >> ~/.zshrc \
    ;fi

# INSTALL PHP BCMATH ##############################################################################

USER root

ARG INSTALL_BCMATH=false
RUN if [ ${INSTALL_BCMATH} = true ]; then \
    apt install -y php-bcmath \
    ;fi

# INSTALL PHP ZIP #################################################################################

USER root

ARG INSTALL_ZIP=false
RUN if [ ${INSTALL_ZIP} = true ]; then \
    apt install -y php-zip \
    ;fi

# INSTALL PHP GD ##################################################################################

USER root

ARG INSTALL_GD=false
RUN if [ ${INSTALL_GD} = true ]; then \
    apt install -y php-gd \
    ;fi

# INSTALL PHP REDIS ###############################################################################

USER root

ARG INSTALL_REDIS=false
RUN if [ ${INSTALL_REDIS} = true ]; then \
    apt install -y php-redis redis-tools \
    ;fi

# INSTALL PHP MONGODB #############################################################################

USER root

ARG INSTALL_MONGODB=false
RUN if [ ${INSTALL_MONGODB} = true ]; then \
    apt install -y php-mongodb mongodb-clients \
    ;fi

# INSTALL PHP MYSQL ###############################################################################

USER root

ARG INSTALL_MYSQL=false
RUN if [ ${INSTALL_MYSQL} = true ]; then \
    apt install -y php-mysql mysql-client \
    ;fi

# INSTALL PHP PGSQL ###############################################################################

USER root

ARG INSTALL_PGSQL=false
RUN if [ ${INSTALL_PGSQL} = true ]; then \
    apt install -y php-pgsql postgresql-client \
    ;fi

# DISABLE OPCACHE #################################################################################

USER root

ARG ENABLE_OPCACHE=true
RUN if [ ${ENABLE_OPCACHE} = false ]; then \
    echo 'opcache.enable=0' >> /etc/php/7.4/cli/conf.d/10-opcache.ini \
    ;fi

# INSTALL PHP XDEBUG ##############################################################################

USER root

ARG INSTALL_XDEBUG=false
RUN if [ ${INSTALL_XDEBUG} = true ]; then \
    apt install -y php-xdebug \
    ;fi

# CLEAN UP ########################################################################################

USER root

RUN apt clean && \
    apt autoremove --purge && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# INIT ############################################################################################

ARG PROJECT_CONTAINER_PATH
WORKDIR ${PROJECT_CONTAINER_PATH}

ENTRYPOINT ["dumb-init", "--"]
CMD ["tail", "-f", "/dev/null"]
