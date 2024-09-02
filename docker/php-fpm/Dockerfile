FROM node:latest AS node
FROM php:8.2-fpm

# Arguments defined in docker-compose.yml
ARG user
ARG uid

ENV TZ "America/Sao_Paulo"

ENV NODE_OPTIONS=--openssl-legacy-provider

#install git and zip
RUN apt-get update && apt-get install libzip-dev zip git wget -y

RUN apt-get update \
    && mkdir tmp && cd ./tmp \
    && docker-php-ext-install pdo_mysql \
    # Install ZLIB
    && apt-get install zlib1g zlib1g-dev \
    && apt-get install -y \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    # Install nano
    && apt-get install nano -y\
    # Install GD
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && apt install -y \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install zip \
    # Install pcntl
    && docker-php-ext-install pcntl\
    # install exif
    &&apt-get install -y libexif-dev \
    && docker-php-ext-install exif \
    && docker-php-ext-enable exif \
    # Clean files
    && cd ../ && rm -rf /var/www/html/tmp \
    && apt-get clean && apt-get autoclean \
    #install composer
    && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer

COPY --from=node /usr/local/lib/node_modules /usr/local/lib/node_modules
COPY --from=node /usr/local/bin/node /usr/local/bin/node
RUN ln -s /usr/local/lib/node_modules/npm/bin/npm-cli.js /usr/local/bin/npm \
    && npm install --global cross-env

# Get latest Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Create system user to run Composer and Artisan Commands
RUN useradd -G www-data,root -u $uid -d /home/$user $user
RUN mkdir -p /home/$user/.composer && \
    chown -R $user:$user /home/$user

RUN pecl install xdebug \
    && docker-php-ext-enable xdebug

USER www-data

ENV PATH="/home/www-data/.composer/vendor/bin:${PATH}"

USER $user
