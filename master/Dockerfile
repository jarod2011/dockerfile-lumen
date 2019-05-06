FROM php:fpm-alpine

MAINTAINER xinghen249@gmail.com

ENV TIMEZONE Asia/Shanghai
ENV PHP_MEMORY_LIMIT 512M
ENV MAX_UPLOAD 200M
ENV PHP_MAX_FILE_UPLOAD 500
ENV PHP_MAX_POST 200M

RUN apk --no-cache add hiredis libmemcached zlib libpng-dev && \
    apk --no-cache add --virtual .build-deps g++ make autoconf hiredis-dev libmcrypt-dev gmp-dev icu-dev linux-headers musl libmemcached-dev cyrus-sasl-dev zlib-dev && \
    cd /root && curl -o /tmp/memcached.tar.gz https://github.com/php-memcached-dev/php-memcached/archive/master.tar.gz -L && \
    tar zxvf /tmp/memcached.tar.gz && cd *memcached* && phpize && \
    ./configure && make && make install && \
    docker-php-ext-enable memcached && \
    cd /root && rm -rf /tmp/memcached* && rm -rf /root/*memcached* && \
    cd /root && curl -o /tmp/xhprof.zip https://github.com/longxinH/xhprof/archive/master.zip -L && \
    cd /tmp/ && unzip /tmp/xhprof.zip && cd /tmp/*xhprof*/extension/ && \
    phpize && ./configure && make && make install && \
    docker-php-ext-enable xhprof && \
    cd  /root && rm -rf /tmp/xhprof* && \
    cd /root && curl -o /tmp/redis.tar.gz https://github.com/phpredis/phpredis/archive/master.tar.gz -L && \
    tar zxvf /tmp/redis.tar.gz && cd *redis* && phpize && \
    ./configure && make && make install && \
    cd /root && rm -rf /tmp/redis* && rm -rf /root/*redis* && \
    docker-php-ext-enable redis && docker-php-ext-install mysqli pdo_mysql zip bcmath gd sockets && \
    php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && \
    php composer-setup.php && \
    php -r "unlink('composer-setup.php');" && \
    mv composer.phar /usr/local/bin/composer && \
    composer self-update && \
    apk del .build-deps .persistent-deps && \
    cd /usr && rm -rf src/ *tar*
    