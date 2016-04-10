---
layout: post
title: ubuntu下建立快捷方式与桌面图标
category : Linux
tagline: "Supporting tagline"
tags : [Linux]
---
{% include JB/setup %}
# ubuntu下建立快捷方式与桌面图标
---

- `cd /usr/share/applicaitons` 此路径存放所有的快捷方式
- 找一个现有的cp，或者仿照一下格式手动建立一个，如给安装在`/usr/local/MATLAB`下的Matlab建立快捷方式：
  + 新建文件：`sudo vi Maltab.desktop`

  ```
  Type=Application  
  Name=Matlab  
  GenericName=Matlab 2015b  
  Comment=Matlab:The Language of Technical Computing  
  Exec=/usr/local/MATLAB/R2015b/bin/matlab  
  Icon=/usr/local/MATLAB/R2015b/Matlab.png  
  Terminal=false  
  StartupNotify=true  
  Categories=Development;Applications;Office;
  ```

  + 说明：主要是`Exec, Icon`，分别对应应用的启动程序和图标。应用的启动程序一般是以应用名命名的文件。
- 建立桌面图标：进入`/usr/share/applicaitons`，然后找到刚刚建立的文件，右击，发送到桌面。
