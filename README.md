# 学习构建自己的镜像

**仅用于开发和学习使用**

## nginx

基于官方 `nginx:xxx-alpine`，调整了软件源，设置了时区`Asia/Shanghai`

## php

基于官方 `php:xxx-fpm-alpine`，调整了软件源，设置了时区，并安装常用扩展 `bcmath`, `pdo_mysql`, `mysqli`, `gd`,`redis`, `mcrypt`

## 感谢

感谢 `dnmp` https://github.com/yeszao/dnmp
