---
title: "使用 Docker 搭建 EFK"
date: 2019-03-14T21:58:00+08:00
tags: ["架构"] 
draft: false
toc: true
---

## 什么是 EFK？

EFK 分别指 Elasticsearch + Fluentd + Kibana，一般用来做日志系统的。他们的作用分别是：

- Elasticsearch：分布式搜索引擎。具有高可伸缩、高可靠、易管理等特点。可以用于全文检索、结构化检索和分析，并能将这三者结合起来。Elasticsearch 基于 Lucene 开发，现在使用最广的开源搜索引擎之一，Wikipedia 、StackOverflow、Github 等都基于它来构建自己的搜索引擎。
- Fluentd：一个消息采集，转化，转发工具，目的是提供中心化的日志服务。
- Kibana：可视化化平台。它能够搜索、展示存储在 Elasticsearch 中索引数据。使用它可以很方便的用图表、表格、地图展示和分析数据。

<!--more-->

随着公司的项目、集群越来越多，统一的日志管理系统是必不可少的。

下面我就分享一下我今天搭建的过程和踩过的坑。

## 安装

自从用过 Docker 之后，只要能用 Docker 的我都用 Docker，能省不少精力和时间，那么安装 EFK 也少，我第一想到的就是去 Google 搜索『Docker efk』，然后找到这个项目 [alextanhongpin/docker-efk](https://github.com/alextanhongpin/docker-efk)，按着下面的命令执行就能安装成功：

```sh
git clone https://github.com/alextanhongpin/docker-efk.git
cd docker-efk
docker-compose up -d elasticsearch fluentd kibana
```

以上三条命令就搭建成功了，方不方便？另外我还想说：

- 查看这个项目的 `docker-compose.yml` 文件，你会发现还有 portainer、dejavu、web 三个服务，用不上其实可以不必安装。如果你想全部安装的话，最后一步就使用 `docker-compose up -d` 命令。
- 如果安装发现有提示端口被占用的情况，你可以改 `docker-compose.yml` 文件之后再运行启动命令就可以了。比方说 web 服务的 80 端口被占用，那么你可以改为 `127.0.0.1:8888:80`，这样本地访问 http://127.0.0.1:8888/ 就可以了。

## 测试

### 本机测试

- 可以使用 docker 安装一个 Ubuntu 环境测试，命令如下：

```
docker run --rm --log-driver=fluentd ubuntu /bin/echo 'Hello world'
```

- 还可以写使用 Python 脚本测试：

```
pip install pyfluent
vim test-efk.py
```

写入代码：

```python
# !/usr/bin/env python
# -*- coding: utf-8 -*-
from pyfluent.client import FluentSender
fluent = FluentSender() # 默认使用 location 和 24224 端口连接 fluentd
fluent.send('Hello pyfluent!')
```

然后运行

```
python test-efk.py
```

### docker 测试

现在的场景是另一个项目在本机的另一个 Docker （下文称 a-docker）里面请求 Fluentd 写入日志，今天这个问题搞了半天都没写入，发现 fluentd 一直是没接收到，也就是说 a-docker 一直没连接到 efk-docker，也就是说：

- a-docker 里面不能 curl 到 127.0.0.1:24224，所以要用宿主主机的 ip
- 通过 `docker ps` 查看 docker-efk_fluentd 的容器只监听了 127.0.0.1 ip 的 24224 端口号，其他 ip 不管。

**所以我们可以简单粗暴的这样解决：**

- 获取本机的 ip 地址
- 修改 a-docker 里面的项目连接 fluentd host 值为本机的 ip
- 修改 efk-docker 的 `docker-compose.yml` 文件，把 fluentd 服务的 ports 改为

```
ports:
    - 0.0.0.0:24224:24224
    - 0.0.0.0:24224:24224/udp
```

**更好的解决方案：**

待补充……

## 为 Fluentd 配置动态 tag

项目之间的日志如何区分？当然是修改 `fluentd.conf` 配置文件的 `logstash_prefix` 来区分，除了为每一个项目手动添加一个 store 配置项的笨方式外，我们还可以通过动态的方式来实现动态标签功能，我修改之后的配置文件如下：

```
<source>
  @type forward
  port  24224
  bind 0.0.0.0
</source>

<filter **.**>
  @type record_transformer
  enable_ruby
  <record>
    tag ${tag}
  </record>
</filter>

# Store data in Elasticsearch
<match *.**>
  @type copy
  <store>
    @type elasticsearch_dynamic
    host docker.for.mac.localhost
    port 9200
    # type_name fluentd
    logstash_format true
    logstash_prefix ${record["tag"]}
    # logstash_prefix fluentd
    # include_tag_key true
    include_tag_key false
    type_name access_log
    tag_key @log_name
  </store>
  <store>
    @type stdout
  </store>
</match>
```

## 最后

总结一下本篇文章，主要介绍：

- EFK 是指 Elasticsearch + Fluentd + Kibana ，一个非常强大统一管理日志的解决方案。
- EFK 使用 Docker 的搭建过程，以及我踩过的坑和解决方案。
- EFK 动态 tag 的配置方式。