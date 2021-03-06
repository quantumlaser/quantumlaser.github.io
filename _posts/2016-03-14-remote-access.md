---
layout: post
title: 基于xrdp和vnc的ubuntu远程桌面
category : Linux
tagline: "Supporting tagline"
tags : [Linux]
---
{% include JB/setup %}
# 基于xrdp和vnc的ubuntu远程桌面
---

## Install
- Reference：<http://jingyan.baidu.com/article/8ebacdf0cdc64949f75cd555.html>
- 安装步骤：

  ```
    sudo apt-get install xrdp vnc4server
    sudo apt-get install xfce4
    echo xfce4-session >~/.xsession
  ```

  + 说明：安装xfce4桌面，因为gnome桌面和xrdp有bug。`~/.xsession`对于每一个远程连接的
用户都需要，因此如果新建立的用户没有此文件，需要手动加入。
  + xrdp主要配置文件：`/etc/xrdp/xrdp.ini, /etc/xrdp/startwm.sh， /etc/xrdp/sesman.ini`

<!--break-->

- 安装完之后(ubuntu 14.04才需要）：
  1. 安装dconf-editor:`sudo apt-get install dconf-editor`
  2. 用Dconf-editor调整，并访问如下配置路径
        `org > gnome > desktop > remote-access`
  3. 取消钩选 “requlre-encryption”属性。
- 搜索“Remote Desktop” 然后更改相应设置(安全->输入密码，可以不勾)：

  ![img](/image/remote/remote.jpg)

## windows下连接
- 在windows下使用“远程桌面进行连接”：module选择`sesman-Xvnc`
,然后输入用户名、密码(可能你的界面里没有port选项，不过没有关系)

  ![img](/image/remote/vnc_login.jpg)

## 会话重连
- 原理：
  + 默认情况下xrdp+vnc在ubuntu 下 每一次连接都打开一个新的session，即使是同一个用户登录。
虽然之前的session并没有退出，但是如果要想回到之前关闭时的界面，需要进行设置。
  + xrdp给每一个session分配了一个端口号，因此如果知道session的port，下次连接的是否更改
port就可以了。
  + Reference:[http://www.bubuko.com/infodetail-924414.html](http://www.bubuko.com/infodetail-924414.html)
- 方法：
  + `sudo vi /etc/xrdp/xrdp.ini`,参考如下修改：

  ```
  [xrdp1]
  name=sesman-Xvnc
  lib=libvnc.so
  username=ask
  password=ask
  ip=127.0.0.1
  port=ask-1
  ```
  + 获取当前会话的port: 打开“远程桌面”，连接的时候，输入用户名、密码的下方多了一个port，默认是-1,如果不更改
登陆的时候就打开一个新的session。查看port的方法有两种：
    * 登陆时会有以下窗口，第六行的`commenting to 127.0.0.1: 5920`，其中5920就是`port`:

    ![img](/image/remote/vnc_port.jpg)

    * 登陆系统后, 在命令行输入：`netstat tunalp | grep vnc`,可以查看当前用户打开的所有连接：

    ![img](/image/remote/vnc_netstat.jpg)

    这样就表示打开了5916-5920 共5个连接。一般最新的一个就是当前打开的连接，如图中的5920。
  + 再次登陆时，将登陆窗口中的port从-1改成前面获取到的port，就可以重新打开这个会话了。

## 关闭会话
- 在远程桌面环境下：点击logout。不过可能有bug。(TODO)
- 或者通过命令行关闭：`netstat tunalp | grep vnc`可以看到当前用户所有登陆的连接，Listen后面的数字是当前进程的pid。
`sudo kill pid`强制关闭会话。

## Tab键不能补全
- 远程桌面中设置，路径是：打开菜单——设置——窗口管理器，或在终端中输入xfwm4-settings打开也行（xfwm4就是xfce4 window manger的缩写），选择键盘，可以看到窗口快捷键中动作一列有“切换同一应用程序的窗口”选项，将该选项的快捷键清除后关闭窗口即可解决问题。

  ![img](/image/remote/xfce_manager.jpg)
