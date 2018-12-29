FROM php:7.2-fpm

RUN apt-get update && apt-get install -y \
	curl \
	wget \
	libfreetype6-dev \
	libjpeg62-turbo-dev \
	libxslt-dev \
	libicu-dev \
	libmcrypt-dev \
	libxml2-dev \
	&& docker-php-ext-install -j$(nproc) iconv mbstring mysqli pdo_mysql zip \
	&& docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
	&& docker-php-ext-install -j$(nproc) gd

RUN pecl install redis-4.0.1 \
    && pecl install xdebug-2.6.0 \
	&& pecl install mcrypt-1.0.1 \
    && docker-php-ext-enable redis xdebug mcrypt
	
RUN docker-php-ext-configure intl
RUN docker-php-ext-install intl
RUN docker-php-ext-install xsl
RUN docker-php-ext-install soap

RUN apt-get install -y iputils-ping telnet

RUN mv "$PHP_INI_DIR/php.ini-development" "$PHP_INI_DIR/php.ini"

ADD ./php.ini /usr/local/etc/php/conf.d/40-custom.ini

ENV PHP_IDE_CONFIG="serverName=Docker"
	
CMD ["php-fpm"]