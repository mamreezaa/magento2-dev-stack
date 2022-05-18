FROM php:8.1-fpm-alpine3.15

ENV USER=app
ENV UID=1000
ENV GID=1000

RUN apk update && apk --no-cache add \
    bash \
    gzip \
    lsof \
    mysql \
    sed \
    libjpeg-turbo-dev \
    zlib-dev \
    libpng-dev \
    freetype-dev \
    icu-dev \
    libxml2-dev \
    libxslt-dev \
    libzip-dev


RUN docker-php-ext-configure \
    gd --with-freetype --with-jpeg \
	&& docker-php-ext-install -j$(nproc) \
    bcmath \
    gd \
    intl \
    pdo_mysql \
    simplexml \
    soap \
    sockets \
    xsl \
    zip

RUN addgroup -g $GID $USER

RUN adduser \
    --disabled-password \
    --gecos "" \
    --home "$(pwd)" \
    --ingroup "$USER" \
    --no-create-home \
    --uid "$UID" \
    "$USER"


RUN mkdir -p /etc/nginx/html /var/www/html /sock \
  && chown -R app:app /etc/nginx /var/www /usr/local/etc/php/conf.d /sock

RUN curl -sS https://getcomposer.org/installer | \
  php -- --install-dir=/usr/local/bin --filename=composer

COPY conf/php.ini $PHP_INI_DIR
COPY conf/php-fpm.conf /usr/local/etc/
COPY conf/www.conf /usr/local/etc/php-fpm.d/

USER app:app
COPY conf/php.ini $PHP_INI_DIR