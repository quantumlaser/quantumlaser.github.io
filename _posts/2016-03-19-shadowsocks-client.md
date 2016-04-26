---
layout: post
title: Shadowsocks 客户端使用
category : Shadowsocks
tagline: "Supporting tagline"
tags : [Linux,Shadowsocks,Net]
---
{% include JB/setup %}
# Shadowsocks客户端使用, 灵活配备代理模式
---

- 在[上一篇](/2016/03/18/shadowsocks-server)中，已经完成了shadowsocks服务端的搭建。
- 不喜欢自己搭建的同学，也可以去shadowsocks的官网或淘宝去购买他们的付费服务。这里买到的就是
一个账号信息
- 本篇希望实现不同平台，包括windows，linux，ios，android，router等的局部和全局的代理，
以及如何实现ipv6下的免流量上网。

<!--break-->

## windows下使用
1.  下载[shadowsocks for Windows](https://github.com/shadowsocks/shadowsocks-windows/releases/download/3.0/Shadowsocks-3.0.zip)。
下载完成后，双击那个小飞机，打开shadowsocks。软件后出现在系统的右下角，找到小飞机，右击，可以看到这样的界面：

    ![img](/image/ss/home.png)

    + 如果勾选上启动系统代理，这样就可以实现全局代理上网了。一般这样过后ie之类的浏览器就可以fq了。
    + 如果想指定代理的方式，点击上图中的系统代理模式，pac表示依据规则，实现gfw墙了得使用代理，
    普通不使用，这样就可以兼顾fq和速度了。如果勾选全局模式，就全部使用代理了。开机启动可以勾上，这样方便经常使用的用户。
    + 最主要的设置来了，点击界面中的服务器，选择编辑服务器。弹出的界面如下：

    ![img](/image/ss/userinfo.png)

    点击左下的添加，然后在右边输入ip，端口，密码，选择加密方式。ip就是前面提到的vps的ip。可以是ipv4的ip也可以是ipv6的ip。选用ipv6的ip时要确保你当前的网络支持ipv6。如果你选取了ipv6，此时通过代理上的网就不会走你的流量了。端口、密码、加密方式都和在上一篇文章中设置保持一致就行。设置好了过后，不要忘了再右击小飞机，在“服务器”里选择你刚刚编辑的服务器。

2. 浏览器插件使用。
如果不想使用前面的系统代理模式，或者如果用的是chrome，建议使用代理插件。推荐使用chrome+SwtichyOmega。这里chrome插件的安装方法我就不赘述了。没有使用过的可以百度。点击添加好的SwtichyOmega，选择“选项”，按照下图设置：

    ![img](/image/ss/switch.png)

你可以新建一个情景模式，名字任意，这里使用shadowsocks。然后在代理协议中选择socks5，服务器和端口如图设置。不代理的地址列表，视个人情况设置。这样设置好了过后，不要忘了点击应用选项。然后在浏览器中，点击SwtichyOmega，选择shadowsocks，就可以走代理上网啦~你可以测试一些国外的网站，比如fb，看是否正常。如果你前面的服务器使用的ipv6的，那么你现在就是免流量上网了。

-  附：Proxifier的使用。
前面的两种方式可能不一定很完美，比如想实现特定的软件，特定的地址，走或不走代理。比如你想实现qq免流量视频，迅雷免流量下载，但是上网走国内，不影响速度。这里推荐使用一个可以在socks5下使用的全局代理软件，Proxifier，软件的下载自行去网上搜索下载。[官网](http://www.proxifier.com/)貌似被墙了。打开这个软件后，点击配置文件，点击代理服务器，打开下面的窗口，如图配置：
    ![img](/image/ss/proxifier.png)

点击确定，完成。这样你所有的软件，包括windows系统服务都将使用shadowsocks的代理了。如果你需要一些软件不使用代理。比如在教育网ipv6下载的utorrent，你可以点击配置文件、代理规则，建立一些直接连接、使用代理连接的规则。这个很简单，大家自行学习一下。
到此，Windows下的shadowsocks代理就配置完成了。你就可以畅享一个免流量又自由上网的环境了。本人做过一些简单的测试，使用ipv6代理的时候，ping值大约在100ms左右，这个值较大，网游可能不太适合，但是普通上网、视频完全没有影响。打开国内网站，稍有变慢。不介意流量的朋友可以再设置国内网站不使用代理，这个是后话了。使用迅雷发现，可以达到慢速10m/s。本人是100M的教育网，平时只在utorrent上看到这个速度，迅雷能有个3-5M就不错了，没想到使用代理能慢速，非常感动。

## Linux下的客户端使用：
- 一般使用Linux的用户都是爱折腾会玩的用户，这里我就不多说了。提醒一下，Linux客户端和服务端是一起安装的。运行ss-server就是做服务器；如果你运行的时ss-local，就可以当客户端用了。运行ss-local的时候也可以像ss-server一样指定配置文件

## 移动端的shadowsocks
- 这里都是安装一些软件就可以了。iphone用户需要越狱，andriod需要root。使用很简单，下载了就能会了。不过可能需要fq

## openwrt路由使用shadowsocks
- Reference:
    + <http://m.oschina.net/blog/487458>
    + <http://www.miui.com/thread-3067556-1-1.html>
