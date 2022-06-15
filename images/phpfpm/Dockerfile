FROM php:8.1-fpm-alpine3.15

ENV USER=app
ENV UID=1000
ENV GID=1000

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


RUN apk update && apk --no-cache add \
    busybox-extras\
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


RUN apk add --no-cache $PHPIZE_DEPS \
    && pecl install xdebug-3.1.0 \
    && docker-php-ext-enable xdebug

RUN curl -sS https://getcomposer.org/installer | \
  php -- --install-dir=/usr/local/bin --filename=composer

COPY conf/php.ini $PHP_INI_DIR
COPY conf/php-fpm.conf /usr/local/etc/
COPY conf/www.conf /usr/local/etc/php-fpm.d/
COPY conf/docker-xdebug.ini /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini


USER app:app
COPY conf/php.ini $PHP_INI_DIR
