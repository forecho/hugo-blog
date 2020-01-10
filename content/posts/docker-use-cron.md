---
title: "Docker 使用定时任务"
date: 2019-05-27T10:58:00+08:00
tags: ["Docker"] 
draft: false
toc: true
---

## 使用定时任务

在宿主主机使用 `crontab -e` 添加如下示例命令就可以了，非常简单：

```
*/15 * * * * docker exec laradock_workspace_1 php /var/www/erp/yii sync/order >> /tmp/out-docker.log 2>&1
```

说明：

- `*/15 * * * *` 代表 15 分钟执行一次
- `>> /tmp/out-docker.log 2>&1` 代表把命令的输出结果输入到 `/tmp/out-docker.log` 文件中。

**需要特别说明的是：**

以往我们进入一个 Docker 终端都带 `-it` 参数，如进入 `laradock_workspace_1` 容器的命令是 `docker exec -it laradock_workspace_1`，但是定时任务的时候不需要这个参数。

> 原因是加了 `-it` 就要开启了一个终端，而计划任务是无法进入任何终端的。

<!--more-->

## 解决定时任务无法工作

定时任务如果没按照我们预期的工作，往往很难排查，但是我们可以通过以下几种方式来快速的定位问题：

### 开启系统 cron 日志

修改系统日志配置信息：

```sh
sudo vim /etc/rsyslog.d/50-default.conf
```

将 cron 前面的注释符去掉，完成之后的代码如下：

```
cron.*  /var/log/cron.log
```

重启 rsyslog

```sh
#sudo /etc/init.d/rsyslog restart
sudo service rsyslog restart
sudo service cron restart
```

然后通过查看 cron 日志文件确认定时任务是否执行：

```sh
sudo tail -n 50 /var/log/cron.log
```

### 命令必须要有输出结果

在我们写脚本的时候，无论成功或者失败都要养成写输出文字的习惯，这样我们就可以方便的使用 `>> /tmp/out-docker.log 2>&1` 的方式把输出结果输入一个日志文件中，我们通过查看这个日志文件进一步确认定时任务的结果。


## 最后

本篇文章主要分享了自己再使用 Docker ，在配置定时任务所踩过的坑，希望对你有用。