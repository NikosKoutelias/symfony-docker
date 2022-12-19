FROM php:8.1-apache

COPY httpd.conf /etc/apache2/sites-enabled/000-default.conf

RUN a2enmod rewrite

RUN apt-get update \
    && apt-get install sudo
    
RUN apt update \
    && apt install -y zlib1g-dev g++ git libicu-dev zip libzip-dev zip libpng-dev libldap2-dev \
    && docker-php-ext-install intl opcache pdo pdo_mysql \
    && pecl install apcu \
    && docker-php-ext-enable apcu \
    && docker-php-ext-configure zip \
    && docker-php-ext-install zip \
    && docker-php-ext-install gd

ADD /custom-php.ini /usr/local/etc/php/conf.d/custom-php.ini

RUN apt-get -y update
RUN apt-get -y install git

RUN curl -sL https://deb.nodesource.com/setup_19.x | bash
RUN apt install -y nodejs

WORKDIR /var/www/html

RUN apt-get purge -y g++ \
    && apt-get autoremove -y \
    && rm -r /var/lib/apt/lists/* \
    && rm -rf /tmp/* 

COPY --from=composer /usr/bin/composer /usr/bin/composer
ENV COMPOSER_ALLOW_SUPERUSER=1

ADD https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/

# RUN chmod +x /usr/local/bin/install-php-extensions && \ 
#     install-php-extensions xdebug

# COPY ./xdebug.ini /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
COPY ./error_reporting.ini /usr/local/etc/php/conf.d/error_reporting.ini

EXPOSE 8080

# Check github connenction and after expose

RUN curl -1sLf 'https://dl.cloudsmith.io/public/symfony/stable/setup.deb.sh' | sudo -E bash
RUN sudo apt install symfony-cli

RUN git config --global user.email "nick.koutelias@gmail.com" \
    && git config --global user.name "Nick Kout"