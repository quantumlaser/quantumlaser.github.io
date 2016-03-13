---
layout: post
title: ubuntu远程桌面
category : Linux
tagline: "Supporting tagline"
tags : [Linux]
---
{% include JB/setup %}
# 基于xrdp和vnc的ubuntu远程桌面
---

- Reference：http://jingyan.baidu.com/article/8ebacdf0cdc64949f75cd555.html
  + 安装步骤：

  ```
    sudo apt-get install xrdp vnc4server
    sudo apt-get install xfce4
    echo xfce4-session >~/.xsession
  ```

  + xrdp配置文件：/etc/xrdp/xrdp.ini, /etc/xrdp/startwm.ini
- 安装完之后(14.04才需要）：
  1. 安装dconf-editor:`sudo apt-get install dconf-editor`
  2. 用Dconf-editor调整，并访问如下配置路径
        `org > gnome > desktop > remote-access`
  3. 取消钩选 “requlre-encryption”属性。
- 搜索“Remote Desktop” 然后更改相应设置：
    ![img](http://blog.onlyforyou.xyz/image/remote.jpg)
- 在windows下使用“远程桌面进行连接”
