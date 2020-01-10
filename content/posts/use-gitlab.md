---
title: "使用 GitLab"
date: 2017-11-07T16:01:00+08:00
tags: ["技术分享", "GitLab"] 
draft: false
toc: true
---

## 安装 GitLab

```
$ sudo apt-get install curl openssh-server ca-certificates postfix
$ curl -sS http://packages.gitlab.cc/install/gitlab-ce/script.deb.sh | sudo bash
$ sudo apt-get install gitlab-ce
```

如果需要安装指定版本，最后一步之前去 <https://packages.gitlab.com/gitlab/gitlab-ce> 看一下，找到指定版本，有安装指南。

PS：查看 Linux 当前系统版本用 `cat /etc/lsb-release` 命令。

修改 GitLab 配置文件

```
$ sudo vim /etc/gitlab/gitlab.rb
```

修改之后的配置如下：

<!--more-->

```
external_url 'http://repo.yd.com';
web_server['external_users'] = ['www-data']
nginx['enable'] = false // 关闭自带nginx
```

**修改时区**

```
$ sudo vim /var/opt/gitlab/gitlab-rails/etc/gitlab.yml
```

修改后为： `time_zone : 'Beijing'`

然后重启 GitLab

```
$ sudo gitlab-ctl reconfigure
$ sudo gitlab-ctl restart
```

## 配置 nginx

GitLab 默认是集成了 nginx，但是我们一般不用他的，上面修改配置的时候，我们已经把 GitLab 集成的 nginx 关闭了。下面我们说一下如何使用 Linux 的 nginx。

添加 gitlab nginx 配置文件

```
$ sudo vim /etc/nginx/conf.d/gitlab.conf
```

添加如下代码：

```
upstream gitlab-workhorse {
  server unix:/var/opt/gitlab/gitlab-workhorse/socket;
}
server {
  listen 80;
  # 此处域名可以根据情况修改，其它地方不用改
  server_name repo.yd.com;
  server_tokens off;
  root /opt/gitlab/embedded/service/gitlab-rails/public;
  access_log  /var/log/nginx/gitlab_access.log;
  error_log   /var/log/nginx/gitlab_error.log;
  location / {
    client_max_body_size 0;
    gzip off;
    proxy_read_timeout      300;
    proxy_connect_timeout   300;
    proxy_redirect          off;
    proxy_http_version 1.1;
    proxy_set_header    Host                $http_host;
    proxy_set_header    X-Real-IP           $remote_addr;
    proxy_set_header    X-Forwarded-For     $proxy_add_x_forwarded_for;
    proxy_set_header    X-Forwarded-Proto   $scheme;
    proxy_pass http://gitlab-workhorse;
  }
}
```

退出 vim， 使用下面命令确认 nginx 配置是否正确：

```
$ sudo nginx -t -c /etc/nginx/nginx.conf
```

重启 nginx

```
$ sudo service nginx reload
```

## 配置 Email

修改配置文件

```
$ sudo vim /etc/gitlab/gitlab.rb
```

依次修改以下几处

```
gitlab_rails['smtp_enable'] = true
gitlab_rails['smtp_address'] = "smtpdm.aliyun.com"
gitlab_rails['smtp_port'] = 25
gitlab_rails['smtp_user_name'] = "gitlab@no-reply.forecho.com"
gitlab_rails['smtp_password'] = "xxxxxx"
gitlab_rails['smtp_domain'] = "no-reply.forecho.com"
gitlab_rails['smtp_authentication'] = "login"
gitlab_rails['smtp_enable_starttls_auto'] = true
gitlab_rails['smtp_tls'] = false
# ...
gitlab_rails['gitlab_email_from'] = "gitlab@no-reply.forecho.com"
```

重启

```
$ sudo gitlab-ctl reconfigure
$ sudo gitlab-ctl restart
```

测试

```
$ sudo gitlab-rails console
// 发送测试邮件
$ Notify.test_email('收件人邮箱', '邮件标题', '邮件正文').deliver_now
```

## 备份

执行备份命令：

```sh
$ sudo gitlab-rake gitlab:backup:create
```

查看备份：

```sh
$ sudo ls /var/opt/gitlab/backups
```

可以通过修改 `/etc/gitlab/gitlab.rb` 的  `gitlab_rails['backup_path']` 来修改默认存放备份文件的目录。


定时任务实现每天凌晨2点进行一次自动备份:

```sh
$ sudo su -
$ crontab -e
```

添加代码 `0 2 * * * /opt/gitlab/bin/gitlab-rake gitlab:backup:create`


## 恢复备份


```sh
# 停止相关数据连接服务
$ sudo gitlab-ctl stop unicorn
$ sudo gitlab-ctl stop sidekiq
# 从1393513186编号备份中恢复
$ sudo gitlab-rake gitlab:backup:restore BACKUP=1393513186
# 启动Gitlab
$ sudo gitlab-ctl start
```

备份相关参考文档[《使用Gitlab一键安装包后的日常备份恢复与迁移》](https://segmentfault.com/a/1190000002439923)。

## 汉化

汉化参考文档[《Omnibus 安装汉化》](https://gitlab.com/xhang/gitlab/wikis/home#omnibus-%E5%AE%89%E8%A3%85%E6%B1%89%E5%8C%96)，自己实践的命令如下：

```sh
$ cd ~
$ git clone https://gitlab.com/xhang/gitlab.git
$ gitlab_version=$(sudo cat /opt/gitlab/embedded/service/gitlab-rails/VERSION)
$ cd gitlab && git diff v${gitlab_version} v${gitlab_version}-zh > ../${gitlab_version}-zh.diff
$ sudo gitlab-ctl stop
$ sudo patch -d /opt/gitlab/embedded/service/gitlab-rails -p1 < ../${gitlab_version}-zh.diff
$ sudo gitlab-ctl start
$ sudo gitlab-ctl reconfigure
```


## GitLab 配置 CI

本来打算安装完 GitLab 之后再安装个 Jenkins 继续集成的，但是发现现在 GitLab 自带 CI 了，虽然没有 Jenkins 强大，但是够我们用。

解决安装源的问题

```
$ sudo curl -L https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh | sudo bash
$ sudo vim /etc/apt/sources.list.d/runner_gitlab-ci-multi-runner.list
```

然后把源换成 https://mirrors.tuna.tsinghua.edu.cn/gitlab-ci-multi-runner/ubuntu/ 清华大学的，然后更新和安装：


```
$ sudo apt-get update && sudo apt-get install gitlab-ci-multi-runner
```

**注册**

```
$ sudo gitlab-ci-multi-runner register
```

url 要添加你自己搭建的 GitLab 域名 + /ci

token 为 GitLab 管理员登陆你自己搭建的 GitLab 之后去 xxx/admin/runners 网址找 token

Please enter the executor: docker-ssh, ssh, docker+machine, kubernetes, docker, parallels, shell, virtualbox, docker-ssh+machine 输入 `shell`

其他的随便填，后期可以在页面上改。


开启和关闭命令

```
$ sudo gitlab-ci-multi-runner start
$ sudo gitlab-ci-multi-runner stop
```

**如何使用？**


这个是重点，在你项目的根目录下新建一个 `.gitlab-ci.yml` 文件。下面是我们使用 angular 实现自动编译的示例：

```
image: node:latest
cache:
  paths:
    - node_modules/

deploy_dev:
  stage: deploy
  environment: Dev
  only:
    - dev
  script:
    - whoami
    - rm ./package-lock.json
    - npm install
    -  ./node_modules/@angular/cli/bin/ng build --prod --build-optimizer
    - ls
    - mv dist company_dev
    - cp -R company_dev /home/www

deploy_test:
  stage: deploy
  environment: Test
  only:
    - test
  script:
    - whoami
    - rm ./package-lock.json
    - npm install
    - ./node_modules/@angular/cli/bin/ng build --prod --env=test --build-optimizer
    - ls
    - mv dist company_test
    - cp -R company_test /home/www
    - echo "发布到测试环境"
    - rsync -avuz -e 'ssh -i /home/www/deploy_rsa' /home/www/company_test root@192.168.1.48:/home/www
    - echo "修改远程目录"
    - ssh -i /home/www/deploy_rsa root@192.168.1.48 "rm -rf /home/www/company && mv /home/www/company_test /home/www/company"
    - curl --request POST --url 'https://oapi.dingtalk.com/robot/send?access_token=xxxxx' --header 'content-type:application/json' --data '{"msgtype":"text","text":{"content":"测试环境已经更新"},"at":{"atMobiles":["xxxx","xxx"],"isAtAll":false}}'
```

简单解释一下：

- 由于之前注册 GitLab CI 的时候选的是 shell，所以运行的时候用的是本台机器的环境，意味着上面的代码你要提前安装好 nodejs 环境。
- GitLab CI 运行的时候使用的是 `gitlab-runner` 用户操作，你如果想实现 `cp` 等一些命令，可能需要把 `gitlab-runner` 添加到有权限的组中。先切换到 root 用户，再切换到  `gitlab-runner` 用户：

```
$ sudo su - 
$ su gitlab-runner
```

- 上面的配置实现的是 `dev` 分支推送代码之后会执行 `deploy_dev` 里面的操作。 `test` 分支推送代码之后会执行 `deploy_test` 里面的操作。
- 最后一行脚本是结合钉钉的[群机器人](https://open-doc.dingtalk.com/docs/doc.htm?treeId=257&articleId=105735&docType=1)做一个回调通知，告诉大家测试环境代码已经更新，非常方便。

## 总结

GitLab 已经是一个互联网企业的标准配置了，免费，功能强大。这篇文章主要是记录自己在公司搭建 GitLab 的过程，包括基本安装，nginx 的配置，配置邮件服务以及 GitLab CI 的使用。
