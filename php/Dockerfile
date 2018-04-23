FROM hub.c.163.com/library/php:7.1-fpm

RUN cp /etc/apt/sources.list /etc/apt/sources.list_bak
RUN echo 'deb http://ftp.cn.debian.org/debian jessie main' > /etc/apt/sources.list
RUN echo 'deb http://ftp.cn.debian.org/debian sid main' >> /etc/apt/sources.list

RUN apt-get update && \
    apt-get remove -y binutils && \
    apt-get install -y gcc git zip unzip php7.1-mysql libpng-dev libfreetype* && \
    php -r "readfile('http://getcomposer.org/installer');" | php -- --install-dir=/usr/bin/ --filename=composer && \
    apt-get -y autoremove && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
 
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/freetype2
RUN docker-php-ext-install gd

RUN pecl install redis
RUN docker-php-ext-enable redis

RUN cp /etc/php/7.1/mods-available/pdo_mysql.ini /usr/local/etc/php/conf.d/ 
RUN cp /usr/lib/php/20160303/pdo_mysql.so /usr/local/lib/php/extensions/no-debug-non-zts-20160303/

WORKDIR /data/www
