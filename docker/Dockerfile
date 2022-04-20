FROM php:7-apache-buster

ENV APACHE_DOCUMENT_ROOT=/var/www/filegator/

RUN apt-get update > /dev/null && \
    # Install and enable php zip extension
    apt-get install -y wget unzip libzip-dev && \
    docker-php-ext-install zip && \
    docker-php-ext-enable zip && \
    # Download and extract latest build
    cd /var/www/ && \
    wget https://github.com/technocalorie/brainfreeze-ftp-server_powered-by-filegator/releases/download/v7.7.2/filegator_v7.7.2.zip && \
    unzip filegator_v7.7.2.zip && rm filegator_v7.7.2.zip && \
    chown -R www-data:www-data filegator/ && \
    chmod -R 775 filegator/ && \
    # configure Apache to use the value of APACHE_DOCUMENT_ROOT as its default Document Root
    sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf && \
    sed -ri -e 's!/var/www/!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf && \
    # configure php
    mv "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini" && \
    # cleanup apt
    apt-get purge -y wget unzip && \
    apt-get autoclean -y && \
    rm -Rf /var/lib/apt/lists/*

EXPOSE 80

VOLUME /var/www/filegator/repository
VOLUME /var/www/filegator/private

WORKDIR "/var/www/filegator/"
