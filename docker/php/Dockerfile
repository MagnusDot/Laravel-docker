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

RUN version=$(php -r "echo PHP_MAJOR_VERSION.PHP_MINOR_VERSION;") \
    && architecture=$(case $(uname -m) in i386 | i686 | x86) echo "i386" ;; x86_64 | amd64) echo "amd64" ;; aarch64 | arm64 | armv8) echo "arm64" ;; *) echo "amd64" ;; esac) \
    && curl -A "Docker" -o /tmp/blackfire-probe.tar.gz -D - -L -s https://blackfire.io/api/v1/releases/probe/php/linux/$architecture/$version \
    && mkdir -p /tmp/blackfire \
    && tar zxpf /tmp/blackfire-probe.tar.gz -C /tmp/blackfire \
    && mv /tmp/blackfire/blackfire-*.so $(php -r "echo ini_get ('extension_dir');")/blackfire.so \
    && printf "extension=blackfire.so\nblackfire.agent_socket=tcp://blackfire:8707\n" > $PHP_INI_DIR/conf.d/blackfire.ini \
    && rm -rf /tmp/blackfire /tmp/blackfire-probe.tar.gz

RUN docker-php-ext-enable apcu
RUN docker-php-ext-install intl opcache
RUN mkdir -p /usr/share/nginx/html/public/upload && chmod -R 747 /usr/share/nginx/html/public/upload
WORKDIR /usr/share/nginx/html
