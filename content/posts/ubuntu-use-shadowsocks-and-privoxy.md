---
title: "Ubuntu 使用 ShadowSocks + Privoxy 代理"
date: 2017-01-07T12:35:00+08:00
tags: ["经验分享", "科学上网"] 
draft: false
toc: true
---
## 引言

先说说这东西有什么用吧，我是 Windows7 使用 Vagrant 安装了个 Ubuntu 虚拟机，然后需要在虚拟机里面配置 SS 代理使用 PHP 的 Composer 下载，不然非常慢。
那么本教程应该同样适用于 Ubuntu 服务器（做为 SS 客户端的方式）配置使用 SS，其他版本的 Linux 要想使用需要稍微改动下，但是思路是一样的。

如果你的 Mac 电脑，终端需要配置 SS 代理，推荐你使用 proxychains-ng 方式，具体查看[macOS 终端走代理（科学上网）](https://gold.xitu.io/entry/5821840cd203090055134cc0)

## ShadowSocks 客户端

注意是 ShadowSocks 客户端，服务端我就不介绍了。先安装 Python pip 再安装 shadowsocks，然后再配置：

```
sudo apt-get install python-pip
sudo pip install shadowsocks
sudo ln -s /usr/local/python/bin/sslocal /usr/bin/sslocal
sudo vim /etc/shadowsocks.conf
```

配置文件代码如下：

```
{
    "server":"your_server_ip",
    "server_port":your_server_port,
    "local_address": "127.0.0.1",
    "local_port":1080, 
    "password":"your_server_passwd",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false,
    "workers": 1 
}
```

**解释**

- `server`: ss服务器IP
- `server_port`: ss服务器IP端口
- `local_address`: 本地ip
- `local_port`:  #本地端口
- `password`: 连接ss密码
- `timeout`: 等待超时
- `method`: 加密方式
- `fast_open`: true 或 false。如果你的服务器 Linux 内核在3.7+，可以开启 fast_open 以降低延迟。开启方法： echo 3 > /proc/sys/net/ipv4/tcp_fastopen 开启之后，将 fast_open 的配置设置为 true 即可
- `workers`: 工作线程数

<!--more-->

开启：

```
# 启动 SS
sudo nohup sslocal -c /etc/shadowsocks.conf >/dev/null 2>%1 &
# 查看进程
sudo ps aux |grep sslocal |grep -v "grep"
```

PS：2018年1月29日更新内容

前一段时间用这种方式发现不用了，搞了半天最后才想起来要看日志，日志大致内容如下：

```
Traceback (most recent call last):
  File "/usr/local/bin/sslocal", line 11, in <module>
    load_entry_point('shadowsocks==2.8.2', 'console_scripts', 'sslocal')()
  File "/usr/local/lib/python2.7/dist-packages/shadowsocks/local.py", line 39, in main
    config = shell.get_config(True)
  File "/usr/local/lib/python2.7/dist-packages/shadowsocks/shell.py", line 262, in get_config
    check_config(config, is_local)
  File "/usr/local/lib/python2.7/dist-packages/shadowsocks/shell.py", line 124, in check_config
    encrypt.try_cipher(config['password'], config['method'])
  File "/usr/local/lib/python2.7/dist-packages/shadowsocks/encrypt.py", line 44, in try_cipher
    Encryptor(key, method)
  File "/usr/local/lib/python2.7/dist-packages/shadowsocks/encrypt.py", line 83, in __init__
    random_string(self._method_info[1]))
  File "/usr/local/lib/python2.7/dist-packages/shadowsocks/encrypt.py", line 109, in get_cipher
    return m[2](method, key, iv, op)
  File "/usr/local/lib/python2.7/dist-packages/shadowsocks/crypto/openssl.py", line 76, in __init__
    load_openssl()
  File "/usr/local/lib/python2.7/dist-packages/shadowsocks/crypto/openssl.py", line 52, in load_openssl
    libcrypto.EVP_CIPHER_CTX_cleanup.argtypes = (c_void_p,)
  File "/usr/lib/python2.7/ctypes/__init__.py", line 378, in __getattr__
    func = self.__getitem__(name)
  File "/usr/lib/python2.7/ctypes/__init__.py", line 383, in __getitem__
    func = self._FuncPtr((name_or_ordinal, self))
AttributeError: /usr/lib/x86_64-linux-gnu/libcrypto.so.1.1: undefined symbol: EVP_CIPHER_CTX_cleanup
```

这是因为 openssl 升级原因导致的，如何解决呢？

看日志找到 `crypto/openssl.py` 所在的文件位置，然后打开文件，此处我的位置为：

```
sudo cp /usr/local/lib/python2.7/dist-packages/shadowsocks/crypto/openssl.py /usr/local/lib/python2.7/dist-packages/shadowsocks/crypto/openssl.py.bak
sudo vim /usr/local/lib/python2.7/dist-packages/shadowsocks/crypto/openssl.py
```

搜索一下 `cleanup`, 一共有两处，把 `cleanup` 改成 `reset`，也就是说:

- 将 `libcrypto.EVP_CIPHER_CTX_cleanup.argtypes = (c_void_p,)` 改为 `libcrypto.EVP_CIPHER_CTX_reset.argtypes = (c_void_p,)`
- 将 `libcrypto.EVP_CIPHER_CTX_cleanup(self._ctx)` 改为 `libcrypto.EVP_CIPHER_CTX_reset(self._ctx)`

详情说明参考文章末尾的链接，然后继续：

```
# 添加开启启动
sudo su
echo "nohup sslocal -c /etc/shadowsocks.conf /dev/null 2>&1 &" >> /etc/rc.local
```


## 安装 Privoxy

```
sudo apt-get install privoxy -y

```

修改配置文件

```
sudo cp /etc/privoxy/config /etc/privoxy/config.bak
sudo vim /etc/privoxy/config
```

找到 `listen-address` 确保有这行代码 `listen-address 127.0.0.1:8118`

找到 `forward-socks5` 确保有这行代码(没有自己加) `forward-socks5  /  127.0.0.1:1080 .`

启动

```
sudo service privoxy start
sudo service privoxy status
```

配置转发

```
sudo vim ~/.bashrc
```

在最后添加如下代码：

```
export http_proxy="http://127.0.0.1:8118"
export https_proxy="http://127.0.0.1:8118"
```

重载配置

```
source ~/.bashrc
```

## 测试

```
curl ip.gs
```

## 参考链接

- [Linux中使用ShadowSocks+Privoxy代理](https://docs.lvrui.io/2016/12/12/Linux%E4%B8%AD%E4%BD%BF%E7%94%A8ShadowSocks-Privoxy%E4%BB%A3%E7%90%86/)
- [linux下的ss+privoxy代理配置](http://www.voidcn.com/blog/xwydq/article/p-5796260.html)
- [Linux 命令行下使用 Shadowsocks 代理](https://mritd.me/2016/07/22/Linux-%E5%91%BD%E4%BB%A4%E8%A1%8C%E4%B8%8B%E4%BD%BF%E7%94%A8-Shadowsocks-%E4%BB%A3%E7%90%86/)
- [解决openssl升级到1.1.0后shadowsocks服务报错问题](https://blog.lyz810.com/article/2016/09/shadowsocks-with-openssl-greater-than-110/)
