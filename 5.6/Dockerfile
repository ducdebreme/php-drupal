# Dockerfile inspired by "official" drupal docker images 
# (@see https://hub.docker.com/_/drupal/)
# 
# from https://www.drupal.org/requirements/php#drupalversions
FROM php:5.6-apache

RUN a2enmod rewrite

# install the PHP extensions we need
RUN apt-get update && apt-get install -y libpng12-dev libjpeg-dev libpq-dev mysql-client openssh-client vim-tiny git \
  && rm -rf /var/lib/apt/lists/* \
  && docker-php-ext-configure gd --with-png-dir=/usr --with-jpeg-dir=/usr \
  && docker-php-ext-install gd mbstring pdo pdo_mysql calendar 

RUN apt-get update && apt-get install -y libmemcached-dev zlib1g-dev libncurses5-dev && \  
  curl -L http://pecl.php.net/get/memcached-2.2.0.tgz >> /usr/src/php/ext/memcached.tgz && \
  tar -xf /usr/src/php/ext/memcached.tgz -C /usr/src/php/ext/ && \
  rm /usr/src/php/ext/memcached.tgz && \
  docker-php-ext-install memcached-2.2.0
  
COPY php.ini /usr/local/etc/php/
