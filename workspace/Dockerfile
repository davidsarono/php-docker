FROM --platform=linux/amd64 phusion/baseimage:master

ARG PHP_VERSION
ENV PHP_VERSION ${PHP_VERSION}

RUN DEBIAN_FRONTEND=noninteractive
RUN locale-gen en_US.UTF-8

ENV LANGUAGE=en_US.UTF-8
ENV LC_ALL=en_US.UTF-8
ENV LC_CTYPE=en_US.UTF-8
ENV LANG=en_US.UTF-8

USER root
RUN apt-get update && \
    apt-get install -y software-properties-common && \
    add-apt-repository -y ppa:ondrej/php
RUN apt-get update && \
    apt-get install -y --allow-downgrades --allow-remove-essential \
        --allow-change-held-packages \
        "php${PHP_VERSION}"-cli \
        "php${PHP_VERSION}"-common \
        "php${PHP_VERSION}"-curl \
        "php${PHP_VERSION}"-mysql \
        "php${PHP_VERSION}"-xml \
        "php${PHP_VERSION}"-intl \
        "php${PHP_VERSION}"-mbstring \
        "php${PHP_VERSION}"-zip \
        "php${PHP_VERSION}"-bcmath \
        "php${PHP_VERSION}"-memcached \
        "php${PHP_VERSION}"-gd \
        "php${PHP_VERSION}"-dev \
        pkg-config \
        libcurl4-openssl-dev \
        libedit-dev \
        libssl-dev \
        libxml2-dev \
        xz-utils \
        git \
        curl \
        vim \
        libgflags-dev \
        libgtest-dev \
        clang \
        libc++-dev \
        protobuf-compiler \
        zlib1g-dev \
    && apt-get clean

RUN curl -s http://getcomposer.org/installer | php && \
    echo "export PATH=${PATH}:/var/www/vendor/bin" >> ~/.bashrc && \
    mv composer.phar /usr/local/bin/composer

RUN . ~/.bashrc

ARG PUID=1000
ARG PGID=1000

ENV PUID ${PUID}
ENV PGID ${PGID}

RUN groupadd -g ${PGID} developer && \
    useradd -u ${PUID} -g developer -m developer && \
    apt-get update -yqq

RUN ln -snf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime && echo Asia/Jakarta > /etc/timezone

COPY ./composer.json /home/developer/.composer/composer.json

RUN chown -R developer:developer /home/developer/.composer

USER developer
RUN composer global install

USER root
RUN pecl channel-update pecl.php.net

COPY ./xdebug.ini /etc/php/${PHP_VERSION}/cli/conf.d/xdebug.ini

RUN pecl install grpc && \
    echo "extension=grpc.so" >> /etc/php/${PHP_VERSION}/mods-available/grpc.ini && \
    phpenmod grpc
RUN pecl install protobuf && \
    echo "extension=protobuf.so" >> /etc/php/${PHP_VERSION}/mods-available/protobuf.ini && \
    phpenmod protobuf
RUN printf "\n" | pecl -q install -o -f redis && \
    echo "extension=redis.so" >> /etc/php/${PHP_VERSION}/mods-available/redis.ini && \
    phpenmod redis

RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

WORKDIR /var/www
