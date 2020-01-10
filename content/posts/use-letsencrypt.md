---
title: "使用 Let’s Encrypt 搭建免费的 HTTPS"
date: 2017-07-14T11:15:00+08:00
tags: ["经验分享"] 
draft: false
toc: true
---

## 1. letsencrypt.sh 证书的生成

### 1.1 目录的生成

```
$ cd ~
$ git clone https://github.com/lukas2511/dehydrated
$ sudo mkdir -p /etc/dehydrated
$ sudo mkdir -p /var/www/dehydrated
$ sudo chown `whoami` -R /var/www/dehydrated
$ sudo chown `whoami` -R /etc/dehydrated
$ cp ~/dehydrated/docs/examples/config /etc/dehydrated/config
$ cp ~/dehydrated/docs/examples/domains.txt /etc/dehydrated/domains.txt
```
<!--more-->

### 1.2 修改 letsencrypt.sh 配置

```
$ vim /etc/dehydrated/config
# 添加如下代码
BASEDIR="/etc/dehydrated/"
WELLKNOWN="/var/www/dehydrated/"
```

修改域名

```
$ vim /etc/dehydrated/domains.txt
# 输入
blog.forecho.com www.forecho.com
```

### 1.3 修改nginx的配置

```
server {
  listen 80;
  ....
  location /.well-known/acme-challenge {
    allow all;
    alias /var/www/dehydrated/;
  }
  ...
}
```

重启 nginx

```
$ service nginx reload
```

### 1.4 执行生成ssl证的脚本

```
$ ~/dehydrated/dehydrated --register --accept-terms
$ ~/dehydrated/dehydrated -c
```

## 2. 配置ssl证到nginx

```
$ sudo openssl dhparam -out /etc/ssl/certs/dhparam.pem 2048
```

### 2.1 nginx 的配置

```
$ sudo vim /etc/nginx/sites-enabled/blog.forecho.com.conf
```

添加 ssl 相关信息

```
server {
  listen 80;
  listen       443 ssl;
  ## listen 443 ssl http2;
  listen       [::]:443 ssl;
  ## ssl
  ssl on;
  ssl_certificate /etc/dehydrated/certs/blog.forecho.com/fullchain.pem;
  ssl_certificate_key /etc/dehydrated/certs/blog.forecho.com/privkey.pem;
  ## ssl pem
  ssl_dhparam /etc/ssl/certs/dhparam.pem;
  ssl_session_cache shared:SSL:10m;
  ssl_session_timeout 10m;
  ssl_protocols TLSv1 TLSv1.1 TLSv1.2;## omit SSLv3 because of POODLE (CVE-2014-3566)
  ssl_stapling on;
  ssl_ciphers "ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA:ECDHE-ECDSA-AES256-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES128-SHA:AES256-SHA:AES:CAMELLIA:DES-CBC3-SHA:!aNULL:!eNULL:!EXPORT:!DES:!RC4:!MD5:!PSK:!aECDH:!EDH-DSS-DES-CBC3-SHA:!EDH-RSA-DES-CBC3-SHA:!KRB5-DES-CBC3-SHA";
  ssl_prefer_server_ciphers on;
  add_header Strict-Transport-Security "max-age=63072000; includeSubdomains; preload";
}
```

### 2.2 测试脚本并重启 nginx

```
$ sudo nginx -t
$ sudo service nginx reload
```


## 3. 添加自动更新的脚本

```
$ mv ~/dehydrated /etc/dehydrated/
$ vim /etc/dehydrated/auto-renew.sh
# 输入
/etc/dehydrated/dehydrated/dehydrated -c
sudo service nginx reload
```

把脚本改为可执行

```
$ chmod 777 /etc/dehydrated/auto-renew.sh
```

把默认的 nano 改成 vim，如果你喜欢 nano 的话跳过这一步。

```
$ vim ~/.selected_editor
# 输入
SELECTED_EDITOR="/usr/bin/vim.tiny"
```

### 3.1 添加日志目录

```
$ mkdir -p /etc/dehydrated/log
```

### 3.2 添加定时任务

```
$ crontab -e
# 添加以下代码
1 0 1 * * /etc/dehydrated/auto-renew.sh >> /etc/dehydrated/log/lets-encrypt.log 2>&1
```

重启 cron 的服务

```
$ sudo service cron restart
```

搞定！

## 4. 后续添加域名

### 4.1 修改域名配置

```
$ vim /etc/dehydrated/domains.txt
```

### 4.2 创建 nginx 域名配置，无 ssl 配置参数，配置信息再加上

```
location /.well-known/acme-challenge {
    allow all;
    alias /var/www/dehydrated/;
}
```

### 4.3 重启 nginx 

```
$ sudo service nginx reload
```

### 4.4 生成证书

```
$ sudo /etc/dehydrated/dehydrated/dehydrated -c
```

### 4.5 修改 nginx 配置信息（添加 ssl 配置参数 ，参照上面2.1），然后再重启 nginx

```
$ sudo service nginx reload
```


**参考链接：** [Let’s Encrypt免费的https证书](http://blog.grayson.org.cn/blog/2016/08/11/letsencrypt/)
