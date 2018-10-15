FROM php:7-fpm-alpine

# change source
RUN cp /etc/apk/repositories /etc/apk/repositories.bak \
	&& echo "http://mirrors.aliyun.com/alpine/v3.7/main/" > /etc/apk/repositories \
	&& echo "http://mirrors.aliyun.com/alpine/v3.7/community/" >> /etc/apk/repositories \
	&& apk update \
	&& apk -U --no-cache add \
        # install pecl extensions dependent
        gcc \
        g++ \
        autoconf \
        make \
        # gd
        libpng-dev \
        libjpeg-turbo-dev \
        libwebp-dev \
        # memcached and others
        zlib-dev \
        libxpm-dev \
        # bz2
        bzip2-dev \
        curl-dev \
        # soap
        libxml2-dev \
        # enchant
        enchant-dev \
        # replace openssl-dev/libressl-dev in 3.7
        libressl2.6-libcrypto \
        # gmp
        gmp-dev \
        # imap
        imap-dev \
        icu-dev \
        # ldap
        openldap-dev \
        # odbc
        #unixodbc \
        #unixodbc-dev \
        # snmp
        net-snmp-dev \
        freetype-dev \
        ncurses-dev \
        # mcrypt
        libmcrypt-dev \
        # xsl
        libxslt-dev \
        libevent-dev \
        flex-dev \
        # pdo_dblib
        freetds-dev \
        # recode
        #recode-dev \
        # snmp
        net-snmp-dev \
        # pspell
        #aspell \
        # tidy
        tidyhtml-dev


# install everything needed  “##” means offical alpine image already include
RUN docker-php-ext-install \
    #########################################
    ## offical alpine image already include
    ##ctype \
    ##curl \
    ##dom \
    ##fileinfo \
    ##filter \
    ##ftp \
    ##hash \
    ##iconv \
    ##json \
    ##mbstring \
    ##pdo \
    ##pdo_sqlite \
    ##phar \
    ##posix \
    ##reflection \
    ##session \
    ##readline \
    ##sodium \
    ##spl \
    ##standard \
    ##tokenizer \
    ##xml \
    ##xmlreader \
    ##simplexml \
    ##xmlwriter \
    #########################################
    bcmath \
    bz2 \
    calendar \
    dba \
    enchant \
    exif \
    gd \
    gettext \
    gmp \
    imap \
    #################################################
    # need to install interbase and specify the path
    #interbase \
    #pdo_firebird \
    #################################################
    intl \
    ldap \
    mysqli \
    ##############################################
    # need to install oracle and specify the path
    #oci8 \
    #pdo_oci \
    ##############################################
    #odbc \
    opcache \
    pcntl \
    pdo_dblib \
    pdo_mysql \
    ####################################################
    # need to install DB2 unixODBC and specify the path
    #pdo_odbc \
    ####################################################
    ##################################################
    # need to install postgresql and specify the path
    #pdo_pgsql \
    #pgsql \
    ##################################################
    ##############################################
    # need to install aspell and specify the path
    #pspell \
    ##############################################
	####################################
    # need to install recode recode-dev
    #recode \
    ####################################
    shmop \
    snmp \
    soap \
    sockets \
    sysvmsg \
    sysvsem \
    sysvshm \
    ##################################
    # need to install tidy
    tidy \
    ##################################
    wddx \
    xmlrpc \
    xsl \
    zend_test \
    zip

# install redis ext
RUN pecl install redis \
    && docker-php-ext-enable redis \
    && rm -rf /tmp/pear/download/*

# install memcached ext
RUN apk -U --no-cache add -y libmemcached-dev \
	&& pecl install memcached \
    && docker-php-ext-enable memcached \
    && rm -rf /tmp/pear/download/*

# install mongodb ext
RUN pecl install mongodb \
    && docker-php-ext-enable mongodb \
    && rm -rf /tmp/pear/download/*

# clean cache
RUN rm -rf /var/cache/apk/*

# install Composer and add china mirror
RUN /usr/local/bin/php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php composer-setup.php \
    && php -r "unlink('composer-setup.php');" \
    && mv composer.phar /bin/composer \
    && composer config -g repo.packagist composer https://packagist.phpcomposer.com
