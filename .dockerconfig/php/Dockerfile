# FROM php:7-alpine
FROM php:fpm

RUN docker-php-ext-install bcmath pdo_mysql

# RUN apk --no-cache add --virtual .composer-rundeps git subversion openssh mercurial tini bash patch make zip unzip

# RUN echo "memory_limit=-1" > "$PHP_INI_DIR/conf.d/memory-limit.ini" \
#  && echo "date.timezone=${PHP_TIMEZONE:-UTC}" > "$PHP_INI_DIR/conf.d/date_timezone.ini"

# RUN apk add --no-cache --virtual .build-deps zlib-dev libzip-dev \
#  && docker-php-ext-configure zip --with-libzip \
#  && docker-php-ext-install -j$(getconf _NPROCESSORS_ONLN) zip \
#  && runDeps="$( \
#     scanelf --needed --nobanner --format '%n#p' --recursive /usr/local/lib/php/extensions \
#     | tr ',' '\n' \
#     | sort -u \
#     | awk 'system("[ -e /usr/local/lib/" $1 " ]") == 0 { next } { print "so:" $1 }' \
#     )" \
#  && apk add --virtual .composer-phpext-rundeps $runDeps \
#  && apk del .build-deps

# ENV COMPOSER_ALLOW_SUPERUSER 1
# ENV COMPOSER_HOME /tmp
# ENV COMPOSER_VERSION 1.8.4

RUN apt-get update && \
    apt-get install -y --no-install-recommends git zip

RUN curl --silent --show-error https://getcomposer.org/installer | php
RUN apt-get update && \
    apt-get install -y --no-install-recommends git zip

RUN mv composer.phar /usr/local/bin/composer 

# WORKDIR /scripts/

# ADD composer.json /scripts/
# ADD composer.lock /scripts/

# RUN apk add --no-cache --virtual .composer-runtime-deps git && \
#     docker-php-composer-install --version=1.2.3 && \
#     composer install --no-dev --optimize-autoloader && \
#     rm $(which composer) && \
#     apk del .composer-runtime-deps

# ADD src/ /scripts/src/

# php oci
# https://github.com/caffeinalab/php-fpm-oci8/blame/master/Dockerfile  ใช้ docker file เวอร์ชั่นเก่า 
LABEL maintainer "Stefano Azzolini <stefano.azzolini@caffeina.com>"

RUN apt-get update && apt-get -y install wget bsdtar libaio1 && \
 wget -qO- https://raw.githubusercontent.com/caffeinalab/php-fpm-oci8/master/oracle/instantclient-basic-linux.x64-12.2.0.1.0.zip | bsdtar -xvf- -C /usr/local && \
 wget -qO- https://raw.githubusercontent.com/caffeinalab/php-fpm-oci8/master/oracle/instantclient-sdk-linux.x64-12.2.0.1.0.zip | bsdtar -xvf-  -C /usr/local && \
 wget -qO- https://raw.githubusercontent.com/caffeinalab/php-fpm-oci8/master/oracle/instantclient-sqlplus-linux.x64-12.2.0.1.0.zip | bsdtar -xvf- -C /usr/local && \
 ln -s /usr/local/instantclient_12_2 /usr/local/instantclient && \
 ln -s /usr/local/instantclient/libclntsh.so.* /usr/local/instantclient/libclntsh.so && \
 ln -s /usr/local/instantclient/lib* /usr/lib && \
 ln -s /usr/local/instantclient/sqlplus /usr/bin/sqlplus && \
 docker-php-ext-configure oci8 --with-oci8=instantclient,/usr/local/instantclient && \
 docker-php-ext-install oci8 && \
 rm -rf /var/lib/apt/lists/* && \
 php -v

VOLUME /etc/tnsnames.ora


# รันได้แต่ไม่ขึ้น

# RUN apt-get update -qq 
# RUN apt-get -y install -qq build-essential unzip wget libaio1 
# RUN mkdir -p /opt/oracle 
# RUN wget https://github.com/bumpx/oracle-instantclient/raw/master/instantclient-basic-linux.x64-12.1.0.2.0.zip 
# RUN wget https://github.com/bumpx/oracle-instantclient/raw/master/instantclient-sdk-linux.x64-12.1.0.2.0.zip 
# RUN unzip -o ./instantclient-basic-linux.x64-12.1.0.2.0.zip -d /opt/oracle 
# RUN unzip -o ./instantclient-sdk-linux.x64-12.1.0.2.0.zip -d /opt/oracle 
# RUN ln -s /opt/oracle/instantclient/sqlplus /usr/bin/sqlplus 
# RUN ln -s /opt/oracle/instantclient_12_1 /opt/oracle/instantclient 
# RUN ln -s /opt/oracle/instantclient/libclntsh.so.12.1 /opt/oracle/instantclient/libclntsh.so 
# RUN ln -s /opt/oracle/instantclient/libocci.so.12.1 /opt/oracle/instantclient/libocci.so

# RUN pecl channel-update pecl.php.net
# RUN echo 'instantclient,/opt/oracle/instantclient' | pecl install oci8 




# ผิดที่ apt-get install php-dev php-pear build-essential libaio1
# RUN apt-get update && apt-get -y install wget bsdtar libaio1
# RUN mkdir /opt/oracle 
# RUN mkdir -p /opt/oracle 
# RUN wget https://github.com/bumpx/oracle-instantclient/raw/master/instantclient-basic-linux.x64-12.1.0.2.0.zip 
# RUN unzip -o ./instantclient-basic-linux.x64-12.1.0.2.0.zip -d /opt/oracle 
# RUN wget https://github.com/bumpx/oracle-instantclient/raw/master/instantclient-sdk-linux.x64-12.1.0.2.0.zip 

# RUN unzip -o ./instantclient-sdk-linux.x64-12.1.0.2.0.zip -d /opt/oracle 
# RUN ln -s /opt/oracle/instantclient_12_1/libclntsh.so.12.1 /opt/oracle/instantclient_12_1/libclntsh.so
# RUN ln -s /opt/oracle/instantclient_12_1/libocci.so.12.1 /opt/oracle/instantclient_12_1/libocci.so
# RUN echo /opt/oracle/instantclient_12_1 > /etc/ld.so.conf.d/oracle-instantclient
# RUN ldconfig
# RUN apt-get install php-dev php-pear build-essential libaio1
# RUN pecl install oci8
# RUN instantclient,/opt/oracle/instantclient_12_1
# RUN echo "extension = oci8.so" >> /etc/php/7.1/fpm/php.ini
# RUN echo "extension = oci8.so" >> /etc/php/7.1/cli/php.ini
# RUN php -m | grep 'oci8'
# RUN service php7.1-fpm restart
