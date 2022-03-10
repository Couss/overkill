FROM php:8.1-fpm

RUN docker-php-ext-install pdo_mysql

ADD https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/

RUN chmod +x /usr/local/bin/install-php-extensions && \
    install-php-extensions apcu intl

# Symfony CLI
RUN rm -rf /var/lib/apt/lists/* \
    && echo 'deb [trusted=yes] https://repo.symfony.com/apt/ /' | tee /etc/apt/sources.list.d/symfony-cli.list \
    && apt-get clean -y \
    && apt-get update -y \
    && apt-get install -y symfony-cli

# Install composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && php composer-setup.php && php -r "unlink('composer-setup.php');" && mv composer.phar /usr/local/bin/composer

RUN { \
        echo 'expose_php = off'; \
        echo 'short_open_tag = off'; \
        echo 'display_errors = 1'; \
    } | tee /usr/local/etc/php/conf.d/security.ini

RUN { \
        echo 'date.timezone = Europe/Paris'; \
    } | tee /usr/local/etc/php/conf.d/symfony.ini

WORKDIR /var/www/html
