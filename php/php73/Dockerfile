FROM php:7.3-fpm-alpine3.9
LABEL maintainer="Onehitieoa <1195775472@qq.com>" version="1.0"

ARG SWOOLE_VERSION=4.4.12
ARG REDIS_VERSION=5.0.0
ARG MONGO_VERSION=1.6.0
ARG XDEBUG_VERSION=2.8.0
ARG MSGPACK_VERSION=2.0.3
ARG XHPROF_VERSION=2.1.0
ARG MEMCACHED_VERSION=3.1.4

COPY ./swoole-${SWOOLE_VERSION}.tgz /tmp/swoole.tgz
COPY ./xdebug-${XDEBUG_VERSION}.tgz /tmp/xdebug.tgz
COPY ./mongodb-${MONGO_VERSION}.tgz /tmp/mongodb.tgz
COPY ./redis-${REDIS_VERSION}.tgz /tmp/redis.tgz
COPY ./xhprof-${XHPROF_VERSION}.tar.gz /tmp/xhprof.tar.gz
COPY ./memcached-${MEMCACHED_VERSION}.tgz /tmp/memcached.tgz
COPY ./composer.phar /bin/composer

#Timezone and lib
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.ustc.edu.cn/g' /etc/apk/repositories \
    && apk add -U tzdata \
    && cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && apk del tzdata

# pgsql issue https://github.com/richarvey/nginx-php-fpm/issues/186  https://github.com/docker-library/php/issues/221
#RUN set -ex \
#    && apk --no-cache add postgresql-dev \
#    && docker-php-ext-install pdo pdo_pgsql \
#    && apk del postgresql-dev \
#    && apk add --upgrade postgresql --update-cache --repository http://dl-3.alpinelinux.org/alpine/edge/main/

RUN set -ex \
    && cd /tmp \
    #&& curl -SL "http://pecl.php.net/get/redis-${REDIS_VERSION}.tgz" -o redis.tgz \
    #&& curl -SL "https://pecl.php.net/get/mongodb-${MONGO_SERSION}.tgz" -o mongodb.tgz \
    #&& curl -SL "https://pecl.php.net/get/xdebug-${MONGO_SERSION}.tgz" -o xdebug.tgz \
    && apk add --no-cache --virtual .phpize-deps $PHPIZE_DEPS \
    && apk --no-cache add postgresql-dev \
    && docker-php-ext-install pdo pdo_pgsql \
    && apk del postgresql-dev \
    && apk add --upgrade postgresql --update-cache --repository http://dl-cdn.alpinelinux.org/alpine/edge/main/ \
    # add lib
    && apk --no-cache add mariadb-client zip unzip libzip-dev libaio hiredis-dev gettext gettext-dev libxml2-dev libxslt \
    libxslt-dev linux-headers libaio-dev openssl-dev openssl libstdc++ librdkafka-dev freetype freetype-dev gifsicle jpegoptim \
    libjpeg-turbo libjpeg-turbo-dev libpng libpng-dev pngquant libmemcached libmemcached-dev curl hiredis-dev\
    # php extension: gd
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/local/jpeg/ --with-png-dir=/usr/include/ \
    && docker-php-ext-install -j$(getconf _NPROCESSORS_ONLN) gd \
    # php extension: opcache
    && docker-php-ext-install opcache \
    # php extension: redis
    && pecl install redis.tgz \
    && docker-php-ext-enable redis \
    # php extension: mongodb
    && pecl install mongodb.tgz \
    && docker-php-ext-enable mongodb \
    # php extension: xdebug
    && pecl install xdebug.tgz \
    && docker-php-ext-enable xdebug \
    # php extension: mysqli
    && docker-php-ext-install mysqli \
    # php extension: pdo_mysql
    && docker-php-ext-install pdo_mysql \
    # php extension: soap
    && docker-php-ext-install soap \
    # php extension: xsl
    && docker-php-ext-install xsl \
    # php extension: xmlrpc
    && docker-php-ext-install xmlrpc \
    # php extension: zip
    && docker-php-ext-install zip \
    # php extension: wddx
    && docker-php-ext-install wddx \
    # php extension: sockets
    && docker-php-ext-install sockets \
    # php extension: shmop
    && docker-php-ext-install shmop \
    # php extension: sysvmsg
    && docker-php-ext-install sysvmsg \
    # php extension:sysvsem
    && docker-php-ext-install sysvsem \
    # php extension: sysvshm
    && docker-php-ext-install sysvshm \
    # php extension: pcntl
    && docker-php-ext-install pcntl \
    # php extension: gettext
    && docker-php-ext-install gettext \
    # php extension: bcmath
    && docker-php-ext-install bcmath \
    # php extension exif
    && docker-php-ext-install exif \
    # php extension mbstring
    && docker-php-ext-install mbstring \
    # php extension calendar
    && docker-php-ext-install calendar \
    # msgpack 扩展下载 pecl本地安装 开启扩展(延迟队列使用减少源数据占用空间)
    && wget http://pecl.php.net/get/msgpack-${MSGPACK_VERSION}.tgz -O /tmp/msgpack.tgz \
    && pecl install /tmp/msgpack.tgz \
    && rm -rf /tmp/msgpack.tgz \
    && docker-php-ext-enable msgpack \
    # php extension:
    # && wget xhprof https://github.com/longxinH/xhprof/archive/v${XHPROF_VERSION}.tar.gz -O /tmp/xhprof.tar.gz \
    && mkdir -p /tmp/xhprof \
    && tar -xf /tmp/xhprof.tar.gz -C /tmp/xhprof --strip-components=1 \
    && rm /tmp/xhprof.tar.gz \
    && ( \
        cd /tmp/xhprof/extension \
        && phpize \
        && ./configure  \
        && make -j$(nproc) \
        && make install \
    ) \
    && rm -r /tmp/xhprof \
    && docker-php-ext-enable xhprof \
    # php extension: memcached
    && cd /tmp \
    && mkdir -p memcached \
    && tar -zxf memcached.tgz -C memcached --strip-components=1 \
    && rm memcached.tgz \
        && ( \
            cd memcached \
            && phpize \
            && ./configure \
            && make -j$(nproc) && make install \
        ) \
        && rm -r memcached \
        && docker-php-ext-enable memcached \
    # php extension: swoole
    && cd /tmp \
    && mkdir -p swoole \
    && tar -zxf swoole.tgz -C swoole --strip-components=1 \
    && rm swoole.tgz \
        && ( \
            cd swoole \
            && phpize \
            && ./configure --enable-swoole-debug --enable-async-redis --enable-mysqlnd --enable-sockets --enable-coroutine --enable-openssl \
            && make -j$(nproc) && make install \
        ) \
        && rm -r swoole \
        && docker-php-ext-enable swoole \
    && rm -rf /var/cache/apk/* \
    # last clearn tmp floder
    && rm -rf /tmp/* \
    && chmod +x /bin/composer && /bin/composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/  \
    && apk del .phpize-deps

# php image's www-data user uid & gid are 82, change them to 1000 (primary user)
# RUN apk --no-cache add shadow && usermod -u 1000 www-data && groupmod -g 1000 www-data

CMD ["php-fpm"]