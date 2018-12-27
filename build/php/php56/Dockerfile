FROM php:5.6-fpm-alpine

# 更改镜像源
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories

# 更改为国内时区
RUN apk add --no-cache tzdata \
    && cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && echo "Asia/shanghai" > /etc/timezone \
    && apk del tzdata \

    # 清除缓存
    && rm -rf /tmp/pear/download/* && rm -rf /tmp/pear/cache/* && rm -rf /var/cache/apk/* && rm -rf /var/lib/apk/*

## PHP 扩展库：http://php.net/manual/zh/extensions.membership.php
## PHP 5.6 官方镜像已包含的模块和扩展
## Core ctype curl date dom ereg fileinfo filter ftp hash iconv json libxml mbstring mhash mysqlnd openssl pcre PDO  pdo_sqlite
## Phar posix readline Reflection session SimpleXML SPL sqlite3 standard tokenizer xml xmlreader xmlwriter zlib

# 安装其他 PHP 5.6 扩展
RUN set -xe; \
    # 可直接使用 docker-php-ext-install安装
    docker-php-ext-install \
        bcmath \
        calendar \
        dba \
        exif \
        mysqli \
        opcache \
        pcntl \
        pdo_mysql \
        shmop \
        sockets \
        sysvmsg \
        sysvsem \
        sysvshm \

    # 清除缓存
    && rm -rf /tmp/pear/download/* && rm -rf /tmp/pear/cache/* && rm -rf /var/cache/apk/* && rm -rf /var/lib/apk/*

# 需要安装依赖库的扩展
RUN set -xe; \
    # 安装依赖库
    apk add --no-cache \
        # bz2
        bzip2-dev \
        # enchant gettext intl(libintl库会在安装enchant时安装)
        enchant-dev \
        # gd
        libpng-dev \
        libjpeg-turbo-dev \
        libwebp-dev \
        freetype-dev \
        libxpm-dev \
        # gmp
        gmp-dev \
        # imap
        imap-dev \
        # intl
        icu-dev \
        # ldap
        openldap-dev \
        # mcrypt
        libmcrypt-dev \
        # pdo_dblib
        freetds-dev \
        # snmp
        net-snmp-dev \
        # soap xmlrpc
        libxml2-dev \
        # zip 扩展
        libzip-dev \
        # xsl
        libxslt-dev \
        # pspell
        aspell-dev \
        # pgsql pdo_pgsql
        postgresql-dev \
        # tidy
        #tidyhtml-dev \
        # recode
        #recode-dev \
    # 安装扩展
    # GD 库配置支持 jpeg，xpm
    && docker-php-ext-configure gd --with-jpeg-dir=shared,/usr --with-xpm-dir=shared,/usr --with-freetype-dir=shared,/usr --enable-gd-native-ttf \
    && docker-php-ext-install gd \
    && docker-php-ext-install \
        bz2 \
        enchant \
        gettext \
        gmp \
        imap \
        intl \
        ldap \
        mcrypt \
        pdo_dblib \
        snmp \
        soap \
        xsl \
        wddx \
        xmlrpc \
        zip \
        pspell \
        pgsql \
        pdo_pgsql \
        # 编译失败
        #tidy \
        # recode 会导致内存问题编译失败,不安装
        #recode \

    # 清除缓存
    && rm -rf /tmp/pear/download/* && rm -rf /tmp/pear/cache/* && rm -rf /var/cache/apk/* && rm -rf /var/lib/apk/*

# odbc pdo_odbc 可能会失败，重试几次
RUN set -ex; \
    docker-php-source extract; \
    { \
        echo '# https://github.com/docker-library/php/issues/103#issuecomment-271413933'; \
        echo 'AC_DEFUN([PHP_ALWAYS_SHARED],[])dnl'; \
        echo; \
        cat /usr/src/php/ext/odbc/config.m4; \
    } > temp.m4; \
    mv temp.m4 /usr/src/php/ext/odbc/config.m4; \
    apk add --no-cache unixodbc-dev; \
    docker-php-ext-configure odbc --with-unixODBC=shared,/usr; \
    docker-php-ext-install odbc; \
    docker-php-source delete; \
    apk add --no-cache unixodbc-dev; \
    docker-php-ext-configure pdo_odbc --with-pdo-odbc=unixodbc,/usr; \
    docker-php-ext-install pdo_odbc; \

    # 清除缓存
    rm -rf /tmp/pear/download/* && rm -rf /tmp/pear/cache/* && rm -rf /var/cache/apk/* && rm -rf /var/lib/apk/*


    # 下面两个个数据库扩展需要指定相应的数据库安装目录 （不常用）
    #################################################
    # need to install interbase and specify the path
    #interbase \
    #pdo_firebird \
    #################################################
    ##############################################
    # need to download oracle-instantclient specify the oracle-instantclient path
    #oci8 \ # php version > 7
    #pdo_oci \
    ##############################################

# pecl安装扩展 xdebug，swoole等 5.6 版本下 需pecl下载手动编译安装
RUN apk add --no-cache --virtual .build-nosql-deps \
    autoconf gcc g++ make; \
    # redis
    pecl install redis && docker-php-ext-enable redis; \
    # mongodb
    pecl install mongodb && docker-php-ext-enable mongodb; \
    # memcache，pecl 安装 memcahced 只支持 php 7 以上版本
    apk add --no-cache zlib-dev && pecl install memcache && docker-php-ext-enable memcache; \
    # 清除编译工具
    apk del .build-nosql-deps; \

    # 清除缓存
    rm -rf /tmp/pear/download/* && rm -rf /tmp/pear/cache/* && rm -rf /var/cache/apk/* && rm -rf /var/lib/apk/*

# composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php composer-setup.php \
    && php -r "unlink('composer-setup.php');" \
    && mv composer.phar /bin/composer \
    # laravel-china更快，或者用这个 https://packagist.phpcomposer.com
    && composer config -g repo.packagist composer https://packagist.laravel-china.org

# 可以更改端口
#EXPOSE 9000
#CMD ["php-fpm"]
