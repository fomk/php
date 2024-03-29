FROM php:7.2-fpm-alpine

ENV TZ Europe/Riga

ENV PHPIZE_DEPS \
		autoconf \
		dpkg-dev dpkg \
		file \
		g++ \
		gcc \
		libc-dev \
		make \
		pkgconf \
		re2c

RUN set -xe \
    && apk add --update \
        ${PHPIZE_DEPS} \
        postgresql-dev \
        mysql-dev \
        zlib-dev \
    && pecl install apcu \
    && docker-php-ext-install pdo pdo_pgsql opcache zip pdo_mysql mysqli exif \
    && docker-php-ext-enable pdo pdo_pgsql opcache zip apcu pdo_mysql mysqli exif \
    && apk del \
        postgresql-libs \
        libsasl \
        db \
        zlib-dev \
        mysql-dev \
        ${PHPIZE_DEPS}

RUN set -xe \
    && apk add --update tzdata \
    && cp /usr/share/zoneinfo/${TZ} /etc/localtime \
    && echo "${TZ}" > /etc/timezone \
    && echo "date.timezone=${TZ}" > /usr/local/etc/php/conf.d/timezone.ini \
    && apk del tzdata

RUN mv "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini"

WORKDIR /var/www/html

EXPOSE 9000

CMD ["php-fpm"]