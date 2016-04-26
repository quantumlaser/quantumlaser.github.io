---
layout: post
title: ubuntu用户管理
category: Linux
tagline: Supporting tagline
tags: [Linux]
---
{% include JB/setup %}
# ubuntu 用户管理
--------------------------------------------------------------------------------

## 新增用户

- adduser:`adduser newuser`
  + 会建立该用户及同名组，并建立/home/newuser文件夹，并执行/etc/skel到/home/newuser的拷贝。
- useradd:`useradd newuser`
  + 仅新增用户，需要手动建立用户home文件夹

<!--break-->

## 权限管理
- usermod: 更改用户的权限

```
-c 备注 加上备注。并会将此备注文字加在/etc/passwd中的第5项字段中
-d 用户主文件夹。指定用户登录所进入的目录，并赋予用户对该目录的的完全控制权
-e 有效期限。指定帐号的有效期限。格式为YYYY-MM-DD，将存储在/etc/shadow
-f 缓冲天数。限定密码过期后多少天，将该用户帐号停用
-g 主要组。设置用户所属的主要组
-a 和-G一起使用，表示给该用户扩展用户组
-G 次要组。设置用户所属的次要组，可设置多组
-M 强制不创建用户主文件夹
-m 强制建立用户主文件夹，并将/etc/skel/当中的文件复制到用户的根目录下
-p 密码。输入该帐号的密码
-s shell。用户登录所使用的shell。* /sbin/nologin就可以禁止登陆*。默认时/bin/sh, 建议改为/bin/bash，否则tab都不能补全
-u uid。指定帐号的标志符user id，简称uid
```

- 给用户增加管理管理员权限：` usermod -a -G adm,sudo newuser`
- 禁止用户登陆：`usermod -s /sbin/nologin newuser`。也可以直接修改`/etc/passwd`文件
## 删除用户
- `userdel -r newuser` 加`-r`会连带用户文件夹一起删除

## 查看用户与组
- `/etc/passwd`存放用户信息,一条典型的记录如下
  `wuzhaohui:x:1000:1000:WuZhaohui,,,:/home/wuzhaohui:/bin/bash`
- `/etc/group` 存放组
