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
- 之前一篇[基于xrdp和vnc的ubuntu远程桌面](/2016/03/13/remote-access)中，已经可以实现会话重连的远程桌面了。不过每次都需要手动记下端口名，比较麻烦，使用并不方便。实际上xrdp最终还是调用了vnc。
- vnc4server本身就可以开启远程桌面，windows下有支持vnc的客户端。
- Reference:[Link](http://www.zhukun.net/archives/7907)
- 如果服务端已经配置好，可以直接跳到**连接使用**一节

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
  + 说明：本人在实验室的服务器上将`.vnc/xstartup`放了一份在`/etc/skel`中，因此如果是通过`adduser`新增的用户就不需要进行配置了。

## 连接使用
**说明**：第1,2两步需要在命令行(Termimal)操作，因此需要登陆到需要使用vnc4server的用户的命令行。windows下可以使用putty。[windows下putty ssh登陆服务器教程](http://jingyan.baidu.com/article/454316ab5dd974f7a7c03a18.html)。也可以直接去服务器手动登陆，然后打开`Terminal`。

1. 首次运行设置密码：如果当前用户首次使用，需要设置一下密码。如果直接进行步骤2，会提示输入密码。
  + 命令行运行`vnc4passwd`，然后根据提示设置6-8的密码。
  + 忘记密码了，也运行一下`vnc4server`，就可以重新输入密码了。
2. 服务端运行：`vnc4server -geometry 1366x768 :1`
  + 也可以把后面的`:1`省略(冒号也要省略),系统会自动分配。这里的1时连接的id，后面会用到。如果省略了`:1`,自动分配的id会在命令执行完返回。
  + `-geometry 1366x768` 用来指定分辨率，也可以不指定
3. 客户端下载：[VNC Viewer](http://www.realvnc.com/download/viewer/)
4. 连接：
  + 运行客户端，在VNC Server栏输入：`ip:port`,其中ip为服务器ip，port为(5900+id)，id为第2步指定的或分配的。如果id为1，则port=5901;id为20，则port=5920。
  ![img](/image/remote/vnc_viewer_login.jpg)
  + 点击Connect，输入在第1步设置的密码，就可以登陆了。
  + 关闭的时候直接关闭窗口，下次直接打开VNC viewer就可以了。
5. 关闭一个连接：关闭vnc viewer并不会真的关闭这个连接，真正关闭它需要在命令行运行：`vnc4erver -kill :1`，其中的`1`是第2步的id。
  + 查看当前用户的所有连接：`ls ~/.vnc`,`.pid`结尾的文件代表了一个活动的连接，文件名中`:`后的数就是id。
