---
title: "自动化部署"
date: 2020-03-18T19:36:14+08:00
tags: ["技术分享", "CI/CD"] 
draft: false
toc: true
---

## 引言

有一个基于 Yii 的小项目，放在 GitHub 私有仓库里。刚开始部署的时候，直接手动 SSH 到服务器上，然后执行 `git pull`，简单粗暴。但是随着时间的推移，现在在3台服务器部署了5套代码。以前的部署方式肯定是不行了，那得累死。

折腾了两天，终于搞定了方案，特来分享经验。

<!--more-->

## 解决方案

### 方案一（基于 GitHub Actions）

我还是挺喜欢 GitHub 的，功能强大。去年免费用户也可以创建私有仓库了，而且我昨天调研的时候才发现 [GitHub Actions](https://github.com/features/actions) 对于私有项目也是有免费额度的，每个月 2000 分钟，对于项目足够应付了。于是跑去看文档，看看怎么玩。

发现 Github Actions 上手真简单，分分钟我就实现了自动构建和部署了，第一版代码如下：

```yml
name: PHP Auto Deploy

on:
  push:
    branches: [ master ]
  pull_request:
    branches: [ master ]

jobs:
  build:

    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v2

      - name: Validate composer.json and composer.lock
        run: composer validate

      - name: Setting composer config
        run: composer config -g github-oauth.github.com ${{ secrets.GITHUB_ONLYREAD_TOKEN }}

      - name: Install dependencies
        run: composer install --prefer-dist --no-progress --no-suggest

      - name: Deploy to Server
        uses: yiier/yii2-base-deploy@master
        with:
          user: root
          host: xxxxx
          path: xxxx
          owner: root
        env:
          DEPLOY_KEY: ${{ secrets.DEPLOY_KEY }}
```

运行的时候，发现问题了，GitHub Actions rsync 到国内服务器速度真的巨慢。100M+ 的文件，硬生生的花了2个多小时，这还怎么玩？？

于是我跑到 [V2EX 发帖求助](https://www.v2ex.com/t/653537)，然后我就试用了一下曲线救国的方案，就是先把代码上传到云存储上，然后在 SSH 到服务器上下载部署，折腾了半天，第二个版本代码来了：


```yml
name: PHP Auto Deploy

on:
  push:
    branches: [ master ]
  pull_request:
    branches: [ master ]

jobs:
  build:

    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@master

      - name: Speed up the packages installation process
        run: composer global require hirak/prestissimo

      - name: Setting composer config
        run: composer config -g github-oauth.github.com ${{ secrets.GITHUB_ONLYREAD_TOKEN }}

      - name: Install dependencies
        run: composer install --prefer-dist --ignore-platform-reqs --no-dev

      - name: Organize files 整理文件
        run: rm -rf .git && rm -rf README.md && zip -r code.zip .

      - name: Install OSS && Upload Code
        uses: manyuanrong/setup-ossutil@v1.0
        with:
          endpoint: "oss-cn-beijing.aliyuncs.com"
          access-key-id: ${{ secrets.OSS_ACCESS_KEY_ID }}
          access-key-secret: ${{ secrets.OSS_ACCESS_KEY_SECRET }}
      - run: ossutil cp -rf code.zip oss://xx-backup

      - name: Execute SSH commmands on remote server
        uses: JimCronqvist/action-ssh@master
        env:
          NAME: "test-deploy"
          USER: "root"
          OSS_ACCESS_KEY_ID: ${{ secrets.OSS_ACCESS_KEY_ID }}
          OSS_ACCESS_KEY_SECRET: ${{ secrets.OSS_ACCESS_KEY_SECRET }}
        with:
          hosts: 'root@xxxxxxxx'
          privateKey: ${{ secrets.DEPLOY_KEY }}
          debug: false
          command: |
            mkdir -p ~/www/tmp
            echo $'\n' "------ DOWNLOAD CODE  -------------------" $'\n'
            sudo rm ~/ossutil64
            wget http://gosspublic.alicdn.com/ossutil/1.6.10/ossutil64
            sudo chmod 755 ~/ossutil64
            ~/ossutil64 config -e oss-cn-beijing.aliyuncs.com -i $OSS_ACCESS_KEY_ID -k $OSS_ACCESS_KEY_SECRET -L CH
            ~/ossutil64 cp oss://xxx-backup/code.zip ~/www/tmp && unzip ~/www/tmp/code ~/www/tmp/code.zip
            cp -rf ~/www/tmp/code ~/www/$NAME
            echo $'\n' "------ CODE READY SUCCESSFUL!!! ---------" $'\n'
```


测试了好多车代码终于写完了，但是跑起来发现还是坑爹，阿里云的 OSS 如果选的是国内的区域上传代码的时候就很慢，如果选的是香港的服务器，解决了上传慢的问题，但是发现下载又很慢（我用的是腾讯云的服务器）。

![坑爹呢这是？](https://i.loli.net/2020/03/18/sbHn1OzC79ILa3V.jpg)

懒得再继续折腾这个方案了，跑去试试第二个方案。


### 方案二（基于 Coding 构建）

太久没登录 [Coding](https://coding.net/) 了，这次登录发现改版了不少。以前是免费会员限额项目个数，现在转型为团队版本了，免费版功能基本上不限制，只限制人数，最多5个人。

Coding 的构建是基于开源的 Jenkins，但是我懒得自己搭建了（没服务器）。一边看[文档](https://help.coding.net/docs/devops/ci/introduce.html)一边写 Jenkinsfile，发现 Jenkins 功能真是强大，有一定的学习门槛。但是最终实现了自己想要的效果，代码参考如下：


```
pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: env.GIT_BUILD_REF]],
                    userRemoteConfigs: [[url: env.GIT_REPO_URL, credentialsId: env.CREDENTIALS_ID]]
                ])
            }
        }
        stage('Prepare') {
            steps {
                sh 'composer --version'
//                 sh 'composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/'
//                 sh 'composer config --list --global'
//                 sh "composer config -g github-oauth.github.com ${ env.GITHUB_ONLYREAD_TOKEN }"
//                 sh 'composer install --prefer-dist --ignore-platform-reqs --no-dev -vvv'
            }
        }

        stage('Build') {
            steps {
                echo '构建中...'
                sh 'rm -rf .git README.md Jenkinsfile'
                sh 'tar -zcf code.tar.gz *'
                echo '构建完成.'
            }
        }

        stage('Deploy Test') {
            when {
                branch 'test'
            }
            steps {
                script {
                    deployCode('test', 'xxx.xxx.xxx.xxx', 'root')
                }
            }
        }

        stage('Deploy Prod') {
            when {
                branch 'master'
            }
            steps {
                script {
                    deployCode('xxx', 'xxx.xxx.xxx.xxx', 'root')
                    deployCode('xxx', 'xxx.xxx.xxx.xxx', 'root')
                }
            }
        }
    }
}


def deployCode(String name, String host, String user) {
    try {
        def remote = [:]
        remote.name = host
        remote.allowAnyHosts = true
        remote.host = host
        remote.user = user
        // 需要先创建一对 SSH 密钥，把私钥放在 CODING 凭据管理，把公钥放在服务器的 `.ssh/authorized_keys`，实现免密码登录
        withCredentials([sshUserPrivateKey(credentialsId: env.DEPLOY_KEY, keyFileVariable: 'id_rsa')]) {
            remote.identityFile = id_rsa
            // SSH 上传文件到远端服务器
            sshPut remote: remote, from: 'code.tar.gz', into: '/tmp/'
            // 解压缩
            sshCommand remote: remote, command: "rm -rf ~/www/tmp/code"
            sshCommand remote: remote, command: "mkdir -p ~/www/tmp/code"
            sshCommand remote: remote, command: "tar -zxf /tmp/code.tar.gz -C ~/www/tmp/code"
            sshCommand remote: remote, command: "mkdir -p ~/www/${name}"
            sshCommand remote: remote, command: "cp -R ~/www/tmp/code/* ~/www/${name}"
            // 项目权限
            sshCommand remote: remote, sudo: true, command: "chown -R ${user}:${user} ~/www/${name}"
            sshCommand remote: remote, sudo: true, command: "chmod 777 -R ~/www/${name}/runtime"
            sshCommand remote: remote, sudo: true, command: "chmod 777 -R ~/www/${name}/web/upload"
            sshCommand remote: remote, sudo: true, command: "chmod 777 -R ~/www/${name}/web/assets"
            // 项目 Composer
            sshCommand remote: remote, command: "docker exec -w /var/www/${name} laradock_workspace_1 composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/"
            sshCommand remote: remote, command: "docker exec -w /var/www/${name} laradock_workspace_1 composer global require hirak/prestissimo"
            sshCommand remote: remote, command: "docker exec -w /var/www/${name} laradock_workspace_1 composer config -g github-oauth.github.com ${ env.GITHUB_ONLYREAD_TOKEN }"
            sshCommand remote: remote, command: "docker exec -w /var/www/${name} laradock_workspace_1 composer install --prefer-dist -v"
            // 项目 Command
            sshCommand remote: remote, command: "docker exec -w /var/www/${name} laradock_workspace_1 php yii migrate --interactive=0"
        }
    } catch(err) {
        echo "${err}"
    }
}
```

用法我就不细说了，大家看代码和参考文档，学起来不难。这里说说使用 Coding 构建的坑：

- 在部署代码之前运行 `composer install` 很慢，即使我使用了阿里云的国内源。所以我暂时放弃部署之前运行 `composer install` 了，改为部署的时候在服务器上运行，两种方案各有千秋。
- Coding 构建是支持 GitHub 仓库的代码的，但是貌似不能自动化部署，反正没自动触发。还有就是用了 GitHub 仓库之后，第一步拉代码的时候有点慢，最后我被迫暂时把代码放在 Coding 了。

## 最后

这两种部署方案，我最后总结一下：

- GitHub Actions 上手快，而且官方还提供了一个 Actions 市场，很方便，但是目前功能确实不如 Jenkins 强大。部署国内服务器网速巨慢，但是官方有提供自己部署 GitHub Actions，我没试。
- 使用 Coding 基于 Jenkins 的构建，缺点是 Jenkins 上手要花点时间，优点是功能强大。项目构建网络慢，但是部署国内服务器网络有优势。

总体来说我还是更喜欢 GitHub 以及其生态的，总感觉国内的用不习惯。所以我的观点是能用 GitHub Actions 方案就尽量用。

- [GitHub Actions Documentation](https://help.github.com/cn/actions)
- [About self-hosted runners](https://help.github.com/en/actions/hosting-your-own-runners/about-self-hosted-runners)
- [Jenkins Pipeline 介绍](https://xuanwo.io/2019/08/30/jenkins-pipeline-intro/)
- [Jenkins 用户手册](https://jenkins.io/zh/doc/)
- [Coding 持续集成（构建）介绍](https://help.coding.net/docs/devops/ci/introduce.html)