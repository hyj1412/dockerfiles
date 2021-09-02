# 从官方基础版本构建
# 镜像地址: https://hub.docker.com/_/php
# 该镜像中php相关的配置文件位于 /usr/local/etc 目录中
ARG PHP_VERSION=7.4
FROM php:${PHP_VERSION}-fpm-alpine
# 官方版本默认安装扩展:
# 可以通过命令 docker run php:7.4-fpm-alpine php -m 查看
#
# Core ctype curl date dom fileinfo filter ftp hash iconv json libxml mbstring mysqlnd openssl pcre
# PDO pdo_sqlite Phar posix readline Reflection session SimpleXML sodium SPL sqlite3 standard
# tokenizer xml xmlreader xmlwriter zlib

# 更换国内的镜像
#
# 1. mirrors.aliyun.com (阿里云)
# 2. mirrors.tuna.tsinghua.edu.cn (清华)
# 3. mirrors.ustc.edu.cn (科大)
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories

# 设置时区
ARG TZ=Asia/Shanghai
RUN apk add --no-cache tzdata \
  && cp "/usr/share/zoneinfo/${TZ}" /etc/localtime \
  && echo "${TZ}" > /etc/timezone

COPY ./php.ini "$PHP_INI_DIR/php.ini"
COPY ./php-fpm.conf /usr/local/etc/php-fpm.d/www.conf

# docker-php-ext-install安装PHP扩展，扩展列表
#
# bcmath bz2 calendar ctype curl dba dom enchant exif fileinfo filter ftp gd gettext
# gmp hash iconv imap interbase intl json ldap mbstring mysqli oci8 odbc opcache pcntl pdo
# pdo_dblib pdo_firebird pdo_mysql pdo_oci pdo_odbc pdo_pgsql pdo_sqlite pgsql phar posix pspell readline
# recode reflection session shmop simplexml snmp soap sockets sodium spl standard sysvmsg sysvsem
# sysvshm tidy tokenizer wddx xml xmlreader xmlrpc xmlwriter xsl zend_test zip
RUN apk add --no-cache $PHPIZE_DEPS libpng libpng-dev libjpeg-turbo libjpeg-turbo-dev openssl-dev libzip libzip-dev libmcrypt libmcrypt-dev \
  && docker-php-ext-install -j$(nproc) bcmath gd mysqli pdo_mysql pcntl sockets shmop zip

# 使用pecl和docker-php-ext-enable安装部分扩展
# mcrypt扩展 https://pecl.php.net/package/mcrypt
# redis扩展 https://pecl.php.net/package/redis
# swoole扩展 https://pecl.php.net/package/swoole
ARG PHP_EXT_MCRYPT_VERSION=1.0.4
ARG PHP_EXT_REDIS_VERSION=5.3.4
ARG PHP_EXT_SWOOLE_VERSION=4.7.0

RUN pecl install mcrypt-${PHP_EXT_MCRYPT_VERSION} \
  && pecl install redis-${PHP_EXT_REDIS_VERSION} \
  && pecl install swoole-${PHP_EXT_SWOOLE_VERSION} \
  && docker-php-ext-enable mcrypt redis swoole
