---
layout: post
title: shadowsocks搭建
category : Shadowsocks
tagline: "Supporting tagline"
tags : [Linux,Shadowsocks,Net]
---
{% include JB/setup %}
# Centos/Ubuntu下搭建Shadowsocks, 配合ipv6实现免流量上网
---

- 原文最初发在本人的CSDN博客：[链接](http://blog.csdn.net/xtdao/article/category/2915525)

## 引言
- 终所周知，G.F*W的存在阻碍了人类的进步，因此fq已成为一种刚需。另外，某些高校，比如鄙人的某技术学校，逆天而行，开起了校园网流量收费时代。鉴于校园网ipv6流量依然较长期内不会收费，搭建一个既能fq，又能使用ipv6免流量上网就好了。
- 目前主流的fq技术有：vpn 和代理。主流的vpn技术详见本人的另一篇blog：[Link](http://blog.onlyforyou.xyz/2016/03/13/VPN-technology)。代理技术也很多，传统的http代理由于不加密不安全，曾经一度很火的GoAgent代理也存在加密问题（最近好像不能用了）。进来基于socks5的shadowsocks代理流行起来。
- 本人基于shadowsocks技术，平台Centos/Ubuntu, 支持ipv4/ipv6双路代理。
- 整理整个过程的步骤：
  + vps选择：搭建shadowsocks的服务器，比如国外，推荐新加坡、西雅图的vps。
  + server端配置
  + client端配置与使用：包括windows，linux，openwrt路由
- shadowsocks的[官网](https://shadowsocks.com/)已经堕落了，之前官方的详细教程没有了。github上部分版本的代码也没有了，说根据规则移除了，不知道是否gfw的触手所及。

## VPS选择
- 本人选取了ramnode，机房位于西雅图，详见本人CSDN博客：[Link](http://blog.csdn.net/xtdao/article/details/44070001)
- ramnode家性价比较高，不过2015年有几个月总是说我滥用，经常关我服务器，到现在也没找出原因，不过后来就没出现过了。Ping值在校园网环境下约150-250ms，较慢。不过上网没有明显延时。在ipv6环境上google几乎零延时，上国内的百度有明显变慢。

## Server端部署Shadowsocks
- 目前Shadowsocks有很多版本，libev, python, go等。其中libev相对而言系统占用小，性能较好。python版github上已被移除，不过可以通过pip安装，后面再说。Server端的系统影响不大，这里以Centos为例。
- Centos下安装Shadowsocks-libev: [Github 链接](https://github.com/shadowsocks/shadowsocks-libev)
  1. 安装必要组件： `yum install build-essential autoconf libtool openssl-devel gcc git -y`
  2. 下载源码编译安装：事先进入待安装路径

  ```
  git clone https://github.com/madeye/shadowsocks-libev.git
  cd shadowsocks-libev
  ./configure
  make && make install
  ```
  3. 还可以通过添加仓库的方式安装，详见github链接。
- shadowsocks-libev服务端运行：有两种方法，一种是命令行，一种是配置文件。推荐配置文件
  + 命令行：`nohup ss-server -s IP地址 -p 端口 -k 密码 -m 加密方式 &`
    * 例：`nohup ss-server -s 192.168.1.1 -p 10101 -k woshimima -m aes-256-cfb &`
    * 说明：这里ip地址用的就是你vps的ip，可以是ipv4地址，也可以是ipv6地址。端口的话可以稍微随意，但不要占用服务器的特殊端口，本人使用过443，8388，8989端口。加密方式推荐上面的这种加密方式。这一条语句就是实现了一个ip、端口组合的shadowsocks进程。nohup表示输出重定向，&表示后台运行，不加也可以，但是区别你试试就知道了。
    * 查看进程：通过ps -A | grep ss-server来查看这个进程，通过其id 将其kill。
    * 多进程：，这样你就可以实现多个ip、端口组合的进程，并且设置不同的密码，这样你可以将你的shadowsocks分享给你的朋友使用。
    * 监听ipv6：通过命令行的方式不能实现ipv4，ipv6的同时监听，需要再开一个ipv6 ip的进程。
    * 开机自启动：`echo "nohup ss-server -s IP地址 -p 端口 -k 密码 -m 加密方式 &" >> /etc/rc.local`
  + 配置文件: `config.json`
    * 新建：`vi /etc/shadowsocks-libev/config.json`, 事实上此文件的路径和文件名可以任意制定，只需要后面保持一致。
    * 添加一下内容：

    ```
    {
      "server":["[::0]","0.0.0.0"],
      "server_port":8989,
      "local_address":"127.0.0.1",
      "local_port":1080,
      "password":"123456789",
      "timeout":600,
      "method":"aes-256-cfb"
    }
    ```
      注意server一行，很多教程写的是`"::"`，但最后发现无法监听ipv6，这是libev的bug，python版就没有问题。其它信息根据个人情况更改，注意local的两个信息最好不要动。
    * 运行： `ss-server -c /etc/shadowsocks-libev/config.json -f /var/run/shadowsocks.pid`。这里通过-f 的方式指定pid使得后台运行。ss-server的输出将重定向到/var/log/messages，可以在这里看着一些结果，启动后可以通过netstat -nlp查看监听的端口。
    * 后台运行：`echo "ss-server -c /etc/shadowsocks-libev/config.json -f /var/run/shadowsocks.pid" >> /etc/rc.local`
- 附：ubuntu下Python版安装运行：
  + 安装： requirepython2.7(<3)。

  ```
  apt-get install python-gevent python-pip
  pip install shadowsocks
  ```

  + 运行：`nohup ssserver -c /usr/local/lib/python2.7/dist-packages/shadowsocks/config.json > log &`
  + 自启：`echo "ssserver -c /usr/local/lib/python2.7/dist-packages/shadowsocks/config.json" >> /etc/rc.local`

  ## 
