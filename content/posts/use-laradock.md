---
title: "使用 Laradock"
date: 2018-11-27T21:03:00+08:00
tags: ["docker", "经验分享"] 
---

## 引言

PHP开发，想用 Docker 但又没太多时间去学，那你可以试试 [Lavadock](https://github.com/laradock/laradock) 项目，它是一套完整的基于 Docker 的 PHP 开发环境。包含了预先打包的 Docker 镜像，所有预先配置都是为了提供一个完美的 PHP 开发环境。

你可以很方便的搭建各种环境，比方说 redis、MongoDB、MySQL、Nginx 等等。

<!--more-->

## 快速使用

- 下载代码

```
git clone https://github.com/laradock/laradock.git --depth=1
```
> depth 用于指定克隆深度，为1即表示只克隆最近一次 commit，可以加快 clone 速度。

- 修改配置

```
cd laradock
cp env-example .env
```

- 修改 `.env` 的 `APP_CODE_PATH_HOST` 的值，相对路径就可以。

比方说我的文件结构是这样的：

```
├── env
│   ├── laradock
├── php
│   ├── 3li3
```
那么我的 `APP_CODE_PATH_HOST=../../php`。

另外需要注意的就是由于 MySQL 5.8 开始连接方式有变化，所以推荐使用 5.7 的版本。也就是说我们要改 `.env` 文件的 MySQL 版本，即 `MYSQL_VERSION=5.7`。

连接的 MySQL 的时候 `DB_HOST=mysql` 而不是 `127.0.0.1`。

- 然后修改添加 nginx 配置，比方说 `nginx/sites/blog.conf`：

```
server {

    listen 80;
    listen [::]:80;

    server_name blog.dev.work;
    root /var/www/blog/public;
    index index.php index.html index.htm;

    location / {
         try_files $uri $uri/ /index.php$is_args$args;
    }

    location ~ \.php$ {
        try_files $uri /index.php =404;
        fastcgi_pass php-upstream;
        fastcgi_index index.php;
        fastcgi_buffers 16 16k;
        fastcgi_buffer_size 32k;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        #fixes timeouts
        fastcgi_read_timeout 600;
        include fastcgi_params;
    }

    location ~ /\.ht {
        deny all;
    }

    location /.well-known/acme-challenge/ {
        root /var/www/letsencrypt/;
        log_not_found off;
    }
}
```

- 修改 `/etc/hosts` 文件，添加：

```
127.0.0.1 blog.dev.work
```

- 然后就可以在 laradock 目录下执行命令开始使用了，示例：

```
docker-compose up -d nginx workspace redis mysql phpmyadmin mongo
```

## 换中国源

- 新建 `php-fpm/sources.list` 文件，添加代码（基本上就是只换域名地址，其他不换）：

```
deb http://mirrors.163.com/debian stretch main
deb http://mirrors.163.com/debian/ stretch-updates main
deb http://mirrors.163.com/debian-security stretch/updates main
```
- 然后修改 `php-fpm/Dockerfile` 文件， 在首部添加：

```
COPY ./sources.list /etc/apt/sources.list
```

## 开启 PHP 扩展

- 修改 `.env` 文件的 workspace 和 php-fpm 部分，把对应的改成 `true`。
- 重新 build `docker-compose build --no-cache php-fpm workspace`
- 重启 docker `docker-compose restart php-fpm workspace`


## 使用 Xdebug

- 修改 `.env` 文件 workspace 和 php-fpm 部分， 把对应的 `WORKSPACE_INSTALL_XDEBUG` 和 `PHP_FPM_INSTALL_XDEBUG` 改成 `true`。
- 修改文件 `laradock/workspace/Dockerfile` (261 行)， 把 `apt-get install -y php${LARADOCK_PHP_VERSION}-xdebug && \` 改成
`apt-get update && apt-get install -y php${LARADOCK_PHP_VERSION}-xdebug && \`
- 修改文件 `laradock/workspace/Dockerfile` （ImageMagick RUN）， 把 `apt-get install -y imagemagick php-imagick \` 改成 `apt-get update && apt-get install -y --force-yes imagemagick php-imagick \`
- 修改 `Laradock/PHP-FPM/xdebug.ini` 和 `Laradock/workspace/xdebug.ini` 两个文件的配置， 并且保持一致。配置参考（以下路径全部是 php-fpm 容器里面的路径，需要手动创建文件夹和文件，最好提前创建）：

```
xdebug.remote_connect_back=1
xdebug.remote_port=9000
xdebug.idekey=PHPSTORM
;
xdebug.remote_autostart=1
xdebug.remote_enable=1
xdebug.cli_color=1
xdebug.profiler_enable=1
xdebug.profiler_output_dir = "/var/www/xdebug/xdebug_profiler"
xdebug.max_nesting_level=250
xdebug.remote_log="/var/www/xdebug/xdebug_docker.log"
;
;内存分析
; xdebug.auto_trace = 1
; xdebug.trace_output_dir = "/var/www/xdebug/xdebug_trace"
; xdebug.trace_format = 0
; xdebug.show_mem_delta = 1
; xdebug.collect_params = 4
; xdebug.collect_return = 1
;
xdebug.remote_handler=dbgp
xdebug.remote_mode=req
;
xdebug.var_display_max_children=-1
xdebug.var_display_max_data=-1
xdebug.var_display_max_depth=-1
```

- 重新 build 并且 重启命令：

```
docker-compose up --build -d php-fpm
docker-compose down && docker-compose up -d nginx workspace redis mysql
```

- 查看配置文件：

```
docker exec -it laradock_php-fpm_1 bash
cat /usr/local/etc/php/conf.d/xdebug.ini
```


## 安装 xhprof 扩展

### 2019年08月06日 更新：

此扩展我已经发了 PR : [feat: Add PHP_FPM_INSTALL_XHPROF as an option to install xhprof extension ](https://github.com/laradock/laradock/pull/2077) 和 [fix: fix install xhprof error](https://github.com/laradock/laradock/pull/2141) 更新到了官方库，使用最新版 Laradock 直接改 `.env` 配置文件就可以了。

---------draft: false
toc: true
---

### 旧版

- 修改 `.env` 和 `env-example` 文件，在 PHP_FPM 处添加配置代码，控制开启或者关闭：

```
# ...
PHP_FPM_INSTALL_XHPROF=true
# ...
```

- 修改 `docker-compose.yml` 文件，在对应的 php-fpm 地方添加代码：

```
- INSTALL_XHPROF=${PHP_FPM_INSTALL_XHPROF}
```


- 在 laradock 目录下添加 `php-fpm/xhprof.ini` 文件，代码示例如下：

```
[xhprof]
; extension=xhprof.so
extension=tideways.so
xhprof.output_dir=/var/www/xhprof
; 不需要自动加载，在程序中控制就行
tideways.auto_prepend_library=0
; 频率设置为100，在程序调用时能改
tideways.sample_rate=100
```


- 然后修改 laradock 目录的 `php-fpm/Dockerfile` 文件，在安装扩展的中间找地方（比方说 MongoDB 下面）添加代码：

```
###########################################################################
# Xhprof:
###########################################################################

ARG INSTALL_XHPROF=false

RUN if [ ${INSTALL_XHPROF} = true ]; then \
    # Install the php xhprof extension 
    if [ $(php -r "echo PHP_MAJOR_VERSION;") = 7 ]; then \
      curl -L -o /tmp/xhprof.tar.gz "https://github.com/tideways/php-xhprof-extension/archive/v4.1.6.tar.gz"; \
    else \ 
      curl -L -o /tmp/xhprof.tar.gz "https://codeload.github.com/phacility/xhprof/tar.gz/master"; \
    fi \
    && mkdir -p xhprof \
    && tar -C xhprof -zxvf /tmp/xhprof.tar.gz --strip 1 \
    && ( \
        cd xhprof \
        && phpize \
        && ./configure \
        && make \
        && make install \
    ) \
    && rm -r xhprof \
    && rm /tmp/xhprof.tar.gz \
;fi

COPY ./xhprof.ini /usr/local/etc/php/conf.d
```

- 重新 build 并且 重启 php-fpm 命令：

```
docker-compose up --build -d php-fpm
docker-compose down && docker-compose up -d nginx workspace redis mysql
```

- 检查是否安装成功：

```
docker exec -it laradock_php-fpm_1 php -m | grep tideways
```

或者
```
docker exec -it laradock_php-fpm_1 php --ri tideways
```

## 修改配置情况

如果单纯的只是改 `.ini` 配置文件，改完之后只需要重新 `build` 对应的服务然后重启所有服务就可以了，不需要 `build --no-cache`，这个太耗时间了。

例如：只是修改了 `php-fpm/xhprof.ini` 文件的参数信息，然后只需要如下操作：

```
docker-compose up --build -d php-fpm
docker-compose down && docker-compose up -d nginx workspace redis mysql
```

## 补充几个常用命令

看 Log 日志命令：

```
docker logs -f laradock_workspace_1
```

进入 xx（比方说 MySQL） 容器命令：

```
docker exec -it laradock_mysql_1 bash
```

进入 workspace 容器

```
docker exec -it laradock_workspace_1 bash
```

停止容器命令：

```
docker-compose down
```

安装和启动命令：

```
docker-compose up -d nginx workspace redis mysql
```


## 写在最后

> 特喜欢 Laradock 官方仓库上的一句话 `Use Docker First And Learn About It Later`,可能你并不清楚 Docker 是什么，更不知道 Laradock 是什么，当然我也一样并不是很了解 Docker，但是就像 Laradock 作者写的这句话先用它，然后再去学习它。

## 参考连接

- [翻译了一下 Laradock 的中文文档](https://laravel-china.org/articles/8105/translated-the-chinese-document-of-laradock)
- [Laradock 官网](http://laradock.io/)