FROM hub.c.163.com/library/php:7.1-fpm

RUN cp /etc/apt/sources.list /etc/apt/sources.list_bak && \
    echo "deb http://ftp.cn.debian.org/debian jessie main\ndeb http://ftp.cn.debian.org/debian sid main" > /etc/apt/sources.list

RUN apt-get update && \
    apt-get remove -y binutils && \
    apt-get install -y gcc git zip unzip && \
    php -r "readfile('http://getcomposer.org/installer');" | php -- --install-dir=/usr/bin/ --filename=composer
 
RUN apt-get install -y libpng-dev libfreetype*  && \
    docker-php-ext-configure gd --with-freetype-dir=/usr/include/freetype2 && \
    docker-php-ext-install gd

RUN pecl install redis && \
    docker-php-ext-enable redis

RUN apt-get install -y php7.1-mysql && \
    cp /etc/php/7.1/mods-available/pdo_mysql.ini /usr/local/etc/php/conf.d/ && \
    cp /usr/lib/php/20160303/pdo_mysql.so /usr/local/lib/php/extensions/no-debug-non-zts-20160303/

RUN apt-get -y autoremove && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

COPY ext/xdebug-2.6.0.tgz /usr/local/src/
RUN cd /usr/local/src && \
    tar zxvf xdebug-2.6.0.tgz && \
    cd xdebug-2.6.0 && \
    phpize && \
    ./configure && \
    make && \
    cp modules/xdebug.so /usr/local/lib/php/extensions/no-debug-non-zts-20160303/

WORKDIR /data/www
