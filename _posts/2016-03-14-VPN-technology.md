---
layout: post
title: VPN技术
category: Net
tagline: Supporting tagline
tags: [VPN, Net]
---
{% include JB/setup %}
# VPN技术
--------------------------------------------------------------------------------

1. pptpd vpn:详见笔记：pptpd vpn.md
    - 不支持ipv6
    - GFW封了1723端口，可以设置端口转发和代理。<http://briteming.blogspot.com/2013/03/pptp-vpn_4.html>
2. SoftEther VPN:<http://mawenjian.net/p/1281.html>
    - 支持ipv4，v6。非常强大，有可视化界面
    - 不排除有后门的可能性，不是很推荐
3. OpenVPN:
    - 有两个版本：
        + 开源版：详见后面几条说明
        + 商业版：2个用户:<https://openvpn.net/index.php/access-server/docs/admin-guides-sp-859543150/howto-installation.html>
            * wget <http://swupdate.openvpn.org/as/openvpn-as-2.0.21-Ubuntu14.amd_64.deb>
            * sudo dpkg -i open...
    - <http://www.open-open.com/lib/view/open1438605096114.html>
    - <https://help.ubuntu.com/community/OpenVPN>
    - <https://www.digitalocean.com/community/tutorials/how-to-set-up-an-openvpn-server-on-ubuntu-14-04>
        + 主要参考本文，已经配置访问内网，但外网访问失败
        + iptables -t nat -A POSTROUTING -s 10.8.0.0/24 -o venet0 -j MASQUERADE
    - <https://docs.ucloud.cn/software/vpn/OpenVPN4Ubuntu.html>
        + 里面提到了保存iptables，并开机加载
4. L2TP：<http://willings.me/article/ipv6-l2tp-vpn/>
5. IKEV2:
    - <http://myitmicroblog.svbtle.com/ikev2-vpn-s2s-ios-and-asa-certificate>
    - <https://quericy.me/blog/699>
6. ShadowVPN
    - 基于shadowsocks的全局VPN，但是ios版没有上线，需要自己编译安装
