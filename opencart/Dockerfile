#
# Dockerfile for opencart
# Credits to <https://store.docker.com/community/images/vimagick/opencart>

FROM php:7.2-apache
MAINTAINER kev <noreply@easypi.pro>

RUN a2enmod rewrite

RUN set -xe \
&& apt-get update \
&& apt-get install -y libpng-dev libjpeg-dev libmcrypt-dev \
&& rm -rf /var/lib/apt/lists/* \
&& docker-php-ext-configure gd --with-png-dir=/usr --with-jpeg-dir=/usr \
&& docker-php-ext-install gd mbstring mysqli zip \
&& pecl install mcrypt-1.0.1 \
&& docker-php-ext-enable mcrypt \
&& pecl install xdebug \
&& echo "zend_extension=$(find /usr/local/lib/php/extensions/ -name xdebug.so)" > /usr/local/etc/php/conf.d/xdebug.ini \
&& echo "xdebug.remote_enable=1" >> /usr/local/etc/php/conf.d/xdebug.ini \
&& echo "xdebug.remote_autostart=1" >> /usr/local/etc/php/conf.d/xdebug.ini

WORKDIR /var/www/html

ENV OPENCART_VER 2.3.0.2
ENV OPENCART_URL https://github.com/opencart/opencart/archive/${OPENCART_VER}.tar.gz
ENV OPENCART_FILE opencart.tar.gz

RUN set -xe \
&& curl -sSL ${OPENCART_URL} -o ${OPENCART_FILE} \
# && echo "${OPENCART_MD5} ${OPENCART_FILE}" | md5sum -c \
&& tar xzf ${OPENCART_FILE} --strip 2 --wildcards '*/upload/' \
&& mv config-dist.php config.php \
&& mv admin/config-dist.php admin/config.php \
&& rm ${OPENCART_FILE} \
&& chown -R www-data:www-data /var/www
