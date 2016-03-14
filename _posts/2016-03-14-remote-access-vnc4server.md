---
layout: post
title: 基于vnc4server的ubuntu远程桌面
category : Linux
tagline: "Supporting tagline"
tags : [Linux]
---
{% include JB/setup %}
# 基于vnc4server的ubuntu远程桌面
---

## 引言
- 之前一篇[基于xrdp和vnc的ubuntu远程桌面](http://blog.onlyforyou.xyz/2016/03/13/remote-access)中，已经可以实现会话重连的远程桌面了。不过每次都需要手动记下端口名，比较麻烦，使用并不方便。实际上xrdp最终还是调用了vnc。
- vnc4server本身就可以开启远程桌面，windows下有支持vnc的客户端。

## 服务端安装
- Install：`sudo apt-get install gnome-panel gnome-settings-daemon metacity nautilus gnome-terminal vnc4server`
- 配置：`vim ~/.vnc/xstartup    #新建文件输入以下内容`

  ```
  #!/bin/sh

  export XKL_XMODMAP_DISABLE=1
  unset SESSION_MANAGER
  unset DBUS_SESSION_BUS_ADDRESS

  [ -x /etc/vnc/xstartup ] && exec /etc/vnc/xstartup
  [ -r $HOME/.Xresources ] && xrdb $HOME/.Xresources
  xsetroot -solid grey
  vncconfig -iconic &

  gnome-session &
  gnome-panel &
  gnome-settings-daemon &
  metacity &
  nautilus &
  gnome-terminal &
  vncconfig -nowin &
  ```

  + 给予可执行权限:`sudo chmod +x ~/.vnc/xstartup`

## 连接使用
1. 首次运行设置密码：如果当前用户首次使用，需要设置一下密码
  命令行运行`vnc4passwd`，然后根据提示设置6-8的密码。
2. 服务端运行、关闭：
  首先需要先通过ssh远程登陆服务器，然后运行：

  ```
  #开启桌面
  $ vnc4server -geometry 1366x768 :1    #也可以把后面的:1省略(冒号也要省略),系统会自动分配

  #杀死一个桌面
  $ vnc4server -kill :1    #必须自着桌面ID号,这里是:1
  ```
3. 客户端下载：[VNC Viewer](http://www.realvnc.com/download/viewer/)
4. 连接：
  + 运行客户端，在VNC Server栏输入：`ip:port`,其中ip为服务器ip，port为(5900+id)，id为第2步指定的或分配的。如果id为1，则port=5901;id为20，则port=5920。

  ！[img](/image/vnc_viewer_login.jpg)
  + 点击Connect，输入在第1步设置的密码，就可以登陆了。
  + 关闭的时候直接关闭窗口，下次直接打开VNC viewer就可以了。
