FROM php:7.4-fpm

RUN apt-get update && apt-get install -y libicu-dev libxslt1.1 libxslt1-dev

#Install composer
RUN apt-get install -y curl git zip unzip && curl -s https://getcomposer.org/installer | php && mv composer.phar /usr/local/bin/composer && composer

# Install Postgre PDO
RUN apt-get install -y libpq-dev \
    && docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
    && docker-php-ext-install pdo pdo_pgsql pgsql xsl

RUN apt-get clean

RUN pecl install apcu
RUN docker-php-ext-enable apcu
RUN docker-php-ext-install intl opcache
RUN mkdir -p /usr/share/nginx/html/public/upload && chmod -R 777 /usr/share/nginx/html/
WORKDIR /usr/share/nginx/html
RUN chmod -R 777 .