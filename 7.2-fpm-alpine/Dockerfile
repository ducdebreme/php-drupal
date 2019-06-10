# Dockerfile inspired by "official" drupal docker images 
# (@see https://hub.docker.com/_/drupal/)
# @see also https://github.com/docker-library/drupal/blob/613b25738ce74aa3a675ca1f70ce9e6e3b26036c/7/fpm-alpine/Dockerfile
# 
# from https://www.drupal.org/requirements/php#drupalversions
#
#   docker build -t php-drupal:7.0-fpm-alpine .
#
FROM php:7.2-fpm-alpine

# env vars for building xdebug
ENV BUILD_DEPS autoconf g++ make
# install the PHP extensions we neeeeeeed
# postgresql-dev is needed for https://bugs.alpinelinux.org/issues/3642
RUN set -ex \
	&& apk add --no-cache --virtual .build-deps \
			$BUILD_DEPS \
		coreutils \
		freetype-dev \
		libjpeg-turbo-dev \
		libpng-dev \
		libxml2-dev \
	&& docker-php-ext-configure gd \
		--with-freetype-dir=/usr/include/ \
		--with-jpeg-dir=/usr/include/ \
		--with-png-dir=/usr/include/ \
	&& docker-php-ext-install -j "$(nproc)" \
		gd \
		opcache \
		pdo_mysql \
		zip \
		calendar \
		soap \
	&& pecl install xdebug-2.7.2 \
	&& docker-php-ext-enable xdebug \
	&& runDeps="$( \
		scanelf --needed --nobanner --format '%n#p' --recursive /usr/local \
			| tr ',' '\n' \
			| sort -u \
			| awk 'system("[ -e /usr/local/lib/" $1 " ]") == 0 { next } { print "so:" $1 }' \
	)" \
	&& apk add --virtual .drupal-phpexts-rundeps $runDeps \
	&& apk del .build-deps

# set recommended PHP.ini settings
# see https://secure.php.net/manual/en/opcache.installation.php
COPY opcache-recommended.ini ./usr/local/etc/php/conf.d/
COPY xdebug.ini ./usr/local/etc/php/conf.d