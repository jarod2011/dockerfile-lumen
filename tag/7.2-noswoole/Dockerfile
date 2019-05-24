FROM php:7.2-fpm-alpine

LABEL maintainer="xinghen249@gmail.com"

ENV TIMEZONE Asia/Shanghai
ENV PHP_MEMORY_LIMIT 512M
ENV MAX_UPLOAD 200M
ENV PHP_MAX_FILE_UPLOAD 500
ENV PHP_MAX_POST 200M

RUN apk --no-cache add hiredis libmemcached zlib libpng-dev curl libzip && \
    apk --no-cache add --virtual .build-deps g++ make autoconf libzip-dev hiredis-dev curl-dev libmcrypt-dev gmp-dev icu-dev linux-headers musl libmemcached-dev cyrus-sasl-dev zlib-dev && \
    cd /root && curl -o /tmp/memcached.tar.gz https://github.com/php-memcached-dev/php-memcached/archive/master.tar.gz -L && \
    tar zxvf /tmp/memcached.tar.gz && cd *memcached* && phpize && \
    ./configure && make && make install && \
    docker-php-ext-enable memcached && \
    cd /root && rm -rf /tmp/memcached* && rm -rf /root/*memcached* && \
    cd /root && curl -o /tmp/xhprof.zip https://github.com/longxinH/xhprof/archive/master.zip -L && \
    cd /tmp/ && unzip /tmp/xhprof.zip && cd /tmp/*xhprof*/extension/ && \
    phpize && ./configure && make && make install && \
    docker-php-ext-enable xhprof && \
    cd /root && rm -rf /tmp/xhprof* && \
    cd /root && curl -o /tmp/msgpack.zip https://github.com/msgpack/msgpack-php/archive/master.zip -L && \
    cd /tmp/ && unzip /tmp/msgpack.zip && cd /tmp/*msgpack*/ && \
    phpize && ./configure && make && make install && \
    docker-php-ext-enable msgpack && \
    cd /root && rm -rf /tmp/msgpack* && \
    cd /root && curl -o /tmp/yar.zip https://github.com/laruence/yar/archive/master.zip -L && \
    cd /tmp/ && unzip /tmp/yar.zip && cd /tmp/*yar*/ && \
    phpize && ./configure --enable-msgpack && make && make install && \
    docker-php-ext-enable yar && \
    cd /root && rm -rf /tmp/yar* && \
    cd /root && curl -o /tmp/yaf.zip https://github.com/laruence/yaf/archive/master.zip -L && \
    cd /tmp/ && unzip /tmp/yaf.zip && cd /tmp/*yaf*/ && \
    phpize && ./configure && make && make install && \
    docker-php-ext-enable yaf && \
    cd /root && rm -rf /tmp/yaf* && \
    cd /root && curl -o /tmp/yac.zip https://github.com/laruence/yac/archive/master.zip -L && \
    cd /tmp/ && unzip /tmp/yac.zip && cd /tmp/*yac*/ && \
    phpize && ./configure && make && make install && \
    docker-php-ext-enable yac && \
    cd /root && rm -rf /tmp/yac* && \
    cd /root && curl -o /tmp/redis.tar.gz https://github.com/phpredis/phpredis/archive/master.tar.gz -L && \
    tar zxvf /tmp/redis.tar.gz && cd *redis* && phpize && \
    ./configure && make && make install && \
    cd /root && rm -rf /tmp/redis* && rm -rf /root/*redis* && \
    docker-php-ext-enable redis && docker-php-ext-install mysqli pdo_mysql zip bcmath gd sockets pcntl && \
    apk del .build-deps && \
    php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && \
    php composer-setup.php && \
    php -r "unlink('composer-setup.php');" && \
    mv composer.phar /usr/local/bin/composer && \
    composer self-update  && \
    cd /usr && rm -rf src/ *tar* && rm -rf /tmp/

RUN mkdir /tmp && chmod 777 /tmp && echo "sys_temp_dir='/tmp'" > /usr/local/etc/php/conf.d/php-setting.ini