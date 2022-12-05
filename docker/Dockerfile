FROM php:8.1-apache

WORKDIR /var/www/html

RUN apt-get update \
    && apt-get install sudo
    
RUN apt update \
    && apt install -y zlib1g-dev g++ git libicu-dev zip libzip-dev zip libpng-dev libldap2-dev \
    && docker-php-ext-install intl opcache pdo pdo_mysql \
    && pecl install apcu \
    && docker-php-ext-enable apcu \
    && docker-php-ext-configure zip \
    && docker-php-ext-install zip \
    && docker-php-ext-install gd \
    && docker-php-ext-configure ldap \
    && docker-php-ext-install ldap


RUN apt-get update \
     && apt-get install -y libzip-dev \
     && docker-php-ext-install zip

RUN apt-get update
RUN apt-get -y install nodejs

RUN npm install 

COPY --from=composer /usr/bin/composer /usr/bin/composer


ADD https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/

RUN chmod +x /usr/local/bin/install-php-extensions && \ 
    install-php-extensions xdebug

COPY ./xdebug.ini /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

EXPOSE 8080

// Check with github connenction and after expose
RUN curl -sS https://get.symfony.com/cli/installer | bash
RUN mv /root/.symfony/bin/symfony /usr/local/bin/symfony
RUN git config --global user.email "nick.koutelias@gmail.com" \
    && git config --global user.name "Nick Kout"
