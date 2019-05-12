FROM ubuntu:latest

RUN apt-get update \ 
&& apt-get -y upgrade 


# 安装 tzdata 
ENV  DEBIAN_FRONTEND=noninteractive
RUN apt-get install -y tzdata
RUN ln -fs /usr/share/zoneinfo/Asia/Chongqing /etc/localtime
RUN dpkg-reconfigure --frontend noninteractive tzdata


# 安装php 
RUN apt-get -y install vim git php-fpm php-curl php-gd php-redis php-memcached php-dev php php-mysql

# 获取安装的php版本
# RUN version=$(php -r '$v = PHP_VERSION;echo substr($v,  0 , 3);')

# 安装swoole
RUN set -ex \
&& version=$(php -r '$v = PHP_VERSION;echo substr($v,  0 , 3);') \
&& printf "yes\nyes\nyes\nyes" | pecl install swoole \
&& echo "extension=swoole.so" > /etc/php/${version}/mods-available/swoole.ini \
&& ln -sT  /etc/php/${version}/mods-available/swoole.ini /etc/php/${version}/fpm/conf.d/20-swoole.ini \
&& ln -sT  /etc/php/${version}/mods-available/swoole.ini /etc/php/${version}/cli/conf.d/20-swoole.ini


# 设置php异常日志
RUN set -ex \ 
&& mkdir -p /var/log/php/ \ 
&& cd /var/log  \
&& chown -R www-data:www-data ./php \
&& version=$(php -r '$v = PHP_VERSION;echo substr($v,  0 , 3);') \
&& sed -i 's/^;error_log.*php_errors.log$/error_log = \/var\/log\/php\/php_errors.log/' /etc/php/${version}/cli/php.ini \
&& sed -i 's/^;error_log.*php_errors.log$/error_log = \/var\/log\/php\/php_errors.log/' /etc/php/${version}/fpm/php.ini

# 与nginx 交互使用端口
RUN set -ex \
&& version=$(php -r '$v = PHP_VERSION;echo substr($v,  0 , 3);') \
&& sed -i 's/^listen.*sock$/listen = 9000/' /etc/php/${version}/fpm/pool.d/www.conf 

# 创建快捷方式
RUN set ex \ 
&& version=$(php -r '$v = PHP_VERSION;echo substr($v,  0 , 3);') \
&& ln -sT /usr/sbin/php-fpm${version} /usr/sbin/php-fpm
 
RUN mkdir -p /run/php

EXPOSE 9000


CMD /usr/sbin/php-fpm -F


 