FROM php:5.6.31-fpm-alpine
RUN echo "http://mirrors.aliyun.com/alpine/v3.4/main" > /etc/apk/repositories \
&& echo "http://mirrors.aliyun.com/alpine/v3.4/community" >> /etc/apk/repositories
RUN apk add --no-cache bash git vim openssl freetype-dev jpeg-dev
#####################################
# gd:
#####################################

# Install the PHP gd library
RUN docker-php-ext-configure gd \
        --enable-gd-native-ttf \
        --with-jpeg-dir=/usr/include \
        --with-freetype-dir=/usr/include && \
    docker-php-ext-install gd

#####################################
# PHP REDIS EXTENSION FOR PHP 7.0 & ZIP
#####################################
USER root
RUN apk update \
&& apk add libmcrypt libmcrypt-dev
RUN apk add --no-cache --virtual .phpize-deps $PHPIZE_DEPS linux-headers \
&& pecl install -o -f redis \
&& docker-php-ext-enable redis \
&& pecl install zip-1.13.5 \
&& docker-php-ext-enable zip \
&& rm -rf /tmp/pear \	
&& apk del .phpize-deps \
&& echo 'always_populate_raw_post_data=-1' > /usr/local/etc/php/conf.d/custom.ini \
&& kill -USR2 1
RUN docker-php-ext-install mcrypt pdo_mysql bcmath mysqli mysql
#####################################
# Composer:
#####################################

# Install composer and add its bin to the PATH.
RUN curl -s http://getcomposer.org/installer | php && \
    mv composer.phar /usr/local/bin/composer && \
	composer config -g repo.packagist composer https://packagist.phpcomposer.com
