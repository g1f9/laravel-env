FROM php:7.2-fpm

# 更新仓库源头
RUN apt-get update -yqq && \
    pecl channel-update pecl.php.net

## install php core extension
RUN apt-get update \
  && apt-get install -y --no-install-recommends \
    curl \
    libmemcached-dev \
    libz-dev \
    libpq-dev \
    libjpeg-dev \
    libpng-dev \
    libfreetype6-dev \
    libssl-dev \
    libmcrypt-dev \
  && rm -rf /var/lib/apt/lists/*

RUN docker-php-ext-install pdo_mysql \
  # Install the PHP pdo_pgsql extention
  && docker-php-ext-install pdo_pgsql \
  # Install the PHP gd library
  && docker-php-ext-configure gd \
    --with-jpeg-dir=/usr/lib \
    --with-freetype-dir=/usr/include/freetype2 && \
    docker-php-ext-install gd

RUN docker-php-ext-install zip \
    && docker-php-ext-install bcmath \
    # && docker-php-ext-install tokenizer \
    #&& apt-get install -y libmagickwand-dev imagemagick && \
    #    pecl install imagick && \
    #    docker-php-ext-enable imagick \
    #&& apt-get install -y zlib1g-dev libicu-dev g++ && \
    #    docker-php-ext-configure intl && \
    #    docker-php-ext-install intl \
    && docker-php-ext-install opcache \
    # && apt-get install -y --force-yes jpegoptim optipng pngquant gifsicle \
    && pecl install -o -f redis \
        &&  rm -rf /tmp/pear \
        &&  docker-php-ext-enable redis
RUN curl -sS https://getcomposer.org/installer | php \
    && mv composer.phar /usr/local/bin/composer \
    && composer config -g repo.packagist composer https://packagist.phpcomposer.com 
RUN apt-get update -yqq && apt-get install cron -y
COPY ./crontab /var/spool/cron/crontabs/root
RUN chmod 0644 /var/spool/cron/crontabs/root
RUN crontab /var/spool/cron/crontabs/root
COPY ./entrypoint.sh /usr/local/bin/

ENTRYPOINT ["entrypoint.sh"]
USER root

# Clean up
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    rm /var/log/lastlog /var/log/faillog

RUN usermod -u 1000 www-data

WORKDIR /var/www

CMD ["php-fpm"]
EXPOSE 9000
