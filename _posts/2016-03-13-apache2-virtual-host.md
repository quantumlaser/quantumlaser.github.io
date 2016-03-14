---
layout: post
title: apache2虚拟主机配置
category: Web
tagline: Supporting tagline
tags: [Linux,Apache2,Web]
---
{% include JB/setup %}
# ubuntu 用户管理
--------------------------------------------------------------------------------

## 虚拟主机
- 参考链接：http://linux.cn/article-3164-1.html
- 为虚拟主机创建目录：

  ```
  cd /var/www/html
  mkdir test
  vi test/index.html
  touch "hello" > test/index.html
  ```

- 从默认配置文件创建虚拟主机配置文件

  ```
  cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/test.conf
  ```

- 编辑test.conf: vi test.conf,更新以下几行

  ```
  ServerAdmin webmaster@localhost
  ServerName test.localhost
  ServerAlias www.test.localhost
  DocumentRoot /var/www/html/test
  ```

- 启用配置文件

  ```
  # sudo a2dissite 000-default.conf
  sudo a2ensite test.conf
  ```

- 重启apache2

  ```
  service apache2 restart
  #如果上面失败，试试下面的
  /etc/init.d/apache2 restart
  ```

## ipv6访问
- apache2 设置ipv6 监听
    + 更改 /etc/apache2/ports.conf 中的Listen *:80，实现v4，v6同时监听
- 添加AAAA解析，支持域名解析为ipv6地址
    + 和A解析添加方式一致
