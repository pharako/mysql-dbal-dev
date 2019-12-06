FROM php:7.4-cli

# Install system packages
RUN apt update && apt install -y zip

RUN docker-php-source extract \
  && apt update \
  && docker-php-ext-install pdo_mysql \
  && docker-php-source delete

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php
RUN mv composer.phar /bin/composer

WORKDIR /app
COPY repos/mysql-dbal /app
COPY docker/php/php.ini /usr/local/etc/php

CMD [ "bash" ]
