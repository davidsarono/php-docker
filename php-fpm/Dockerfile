ARG PHP_VERSION

FROM arm64v8/php:${PHP_VERSION}-fpm

RUN apt-get update && \
  apt-get install -y  tzdata \
    build-essential \
    openssl \
    libssl-dev \
    libfreetype6-dev \
    libpng-dev \
    libjpeg62-turbo-dev \
    xml-core \
    redis-tools \
    mcrypt \
    libmcrypt-dev \
    libzip-dev \
    zip \
  && rm -rf /var/lib/apt/lists/*

# Install the PHP pdo_mysql
RUN docker-php-ext-install pdo_mysql && \
# Install the PHP gd library
  docker-php-ext-configure gd --with-freetype --with-jpeg && \
  NPROC=$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1) && \
  docker-php-ext-install -j${NPROC} gd &&\
# Install the PHP redis
  pecl install -o -f redis && \
  docker-php-ext-enable redis && \
# Install the PHP grpc
  pecl install grpc && \
  docker-php-ext-enable grpc && \
# Install the PHP protobuf
  pecl install protobuf && \
  docker-php-ext-enable protobuf && \
# Install the PHP xdebug
  pecl install xdebug && \
  docker-php-ext-enable xdebug && \
# Install the PHP sockets
  docker-php-ext-install sockets && \
# Cleaning up
  rm -rf /tmp/pear

# COPY xdebug config
COPY ./xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

# Set Timezone
RUN ln -snf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime && echo Asia/Jakarta > /etc/timezone

COPY config/php-fpm.conf /usr/local/etc/
COPY config/php.ini /usr/local/etc/php/
COPY config/www.conf /usr/local/etc/php-fpm.d/

RUN usermod -u 1000 www-data

WORKDIR /var/www

CMD ["php-fpm"]

EXPOSE 9000
