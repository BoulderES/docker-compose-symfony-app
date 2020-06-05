FROM php:7.4.6-fpm-alpine3.11

# Set working directory
WORKDIR /var/www

# Install common dependencies
RUN set -ex \
  && apk --no-cache add \
  postgresql-dev \
  mariadb-client \
  libpng-dev \
  libjpeg-turbo-dev \
  freetype-dev \
  rabbitmq-c rabbitmq-c-dev \
  zip \
  jpegoptim optipng pngquant gifsicle \
  vim \
  unzip \
  git \
  curl \
  exiftool \
  oniguruma-dev \
  libzip-dev \
  php7-pecl-amqp \
  php7-pecl-apcu

# Sockets
RUN docker-php-ext-install sockets
# Amqp
RUN apk add --no-cache --update rabbitmq-c-dev \
  && apk add --no-cache --update --virtual .phpize-deps $PHPIZE_DEPS \
  && pecl install -o -f amqp \
  && docker-php-ext-enable amqp \
  && apk del .phpize-deps

# Install common extensions
RUN docker-php-ext-configure exif
RUN docker-php-ext-install exif
RUN docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql
RUN docker-php-ext-install pdo_mysql mbstring zip exif pcntl pdo pdo_pgsql
RUN docker-php-ext-configure gd
RUN docker-php-ext-install gd
RUN docker-php-ext-configure zip

RUN docker-php-ext-enable exif

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Expose port 9000 and start php-fpm server
EXPOSE 9000
CMD ["php-fpm"]