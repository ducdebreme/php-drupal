# Dockerfile inspired by "official" drupal docker images 
# (@see https://hub.docker.com/_/drupal/)
# 
# from https://www.drupal.org/requirements/php#drupalversions
FROM php:5.4-apache

RUN a2enmod rewrite

# install the PHP extensions we need
RUN apt-get update && apt-get install -y libpng12-dev libjpeg-dev libpq-dev mysql-client openssh-client vim-tiny git \
  && rm -rf /var/lib/apt/lists/* \
  && docker-php-ext-configure gd --with-png-dir=/usr --with-jpeg-dir=/usr \
  && docker-php-ext-install gd mbstring pdo pdo_mysql 

RUN apt-get update && apt-get install -y libmemcached-dev zlib1g-dev libncurses5-dev && \  
  curl -L http://pecl.php.net/get/memcached-2.2.0.tgz >> /usr/src/php/ext/memcached.tgz && \
  tar -xf /usr/src/php/ext/memcached.tgz -C /usr/src/php/ext/ && \
  rm /usr/src/php/ext/memcached.tgz && \
  docker-php-ext-install memcached-2.2.0

# xhprof
RUN cd /tmp/ && \
    curl -O https://pecl.php.net/get/xhprof-0.9.4.tgz && \
    tar zxvf xhprof-0.9.4.tgz && \
    mv xhprof-0.9.4/extension /usr/src/php/ext/xhprof && \
    docker-php-ext-install xhprof && \
    RUN echo "xhprof.output_dir=/tmp" >> /usr/local/etc/php/conf.d/docker-php-ext-xhprof.ini 

COPY php.ini /usr/local/etc/php/
