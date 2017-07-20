# Dockerfile inspired by "official" drupal docker images 
# (@see https://hub.docker.com/_/drupal/)
# 
# from https://www.drupal.org/requirements/php#drupalversions
FROM php:7.0-apache

RUN a2enmod rewrite

# install the PHP extensions we need
RUN apt-get update && apt-get install -y libpng12-dev libjpeg-dev libpq-dev libxml2 \
  && rm -rf /var/lib/apt/lists/* \
  && docker-php-ext-configure gd --with-png-dir=/usr --with-jpeg-dir=/usr \
  && docker-php-ext-install gd mbstring pdo pdo_mysql zip calendar soap

RUN apt-get update && apt-get install -y libmemcached-dev git \
    && cd /usr/local \
    && git clone https://github.com/php-memcached-dev/php-memcached \
    && cd php-memcached \
    && git checkout php7 \
    && phpize \
    && PATH=/opt/sp/php7.0/bin:$PATH ./configure --disable-memcached-sasl \
    && make \
    && make install \
    && docker-php-ext-enable memcached

RUN apt-get install -y mysql-client openssh-client vim-tiny git

COPY php.ini /usr/local/etc/php/
