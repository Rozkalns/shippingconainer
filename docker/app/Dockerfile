FROM alpine:3.8

MAINTAINER Roberts Rozkalns <roberts.rozkalns@me.com>

RUN apk add --no-cache \
    gnupg tzdata \
    && ln -snf /usr/share/zoneinfo/UTC /etc/localtime \
    && echo "UTC" > /etc/timezone \
    && apk del tzdata

RUN apk add --no-cache \
    curl zip unzip git supervisor sqlite \
    nginx php7-fpm php7-cli \
    php7-pgsql php7-sqlite3 php7-gd \
    php7-curl php7-memcached \
    php7-imap php7-pdo php7-pdo_mysql php7-mbstring \
    php7-xml php7-zip php7-bcmath php7-soap \
    php7-intl php7-xdebug \
    php7-phar php7-json \
    && php -r "readfile('http://getcomposer.org/installer');" | php -- --install-dir=/usr/bin/ --filename=composer \
    && for dir in php nginx; do mkdir -p /run/${dir}; done

# Add user
RUN set -x ; \
  addgroup -g 82 -S www-data ; \
  adduser -u 82 -D -S -G www-data www-data

# Nginx configuration
RUN rm /etc/nginx/conf.d/*
ADD nginx.conf /etc/nginx/nginx.conf
ADD nginx-vhost.conf /etc/nginx/sites-enabled/app.conf

# PHP configuration
ADD php-env-vars.conf /etc/php7/php-env-vars.conf
ADD php-fpm.conf /etc/php7/php-fpm.conf
ADD php.ini /etc/php7/php.ini

# Configure supervisord
ADD supervisord.conf /etc/supervisor/conf.d/supervisord.conf

CMD ["supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]
