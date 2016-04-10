---
layout: post
title: ubuntu下配置sublime中文与链接
category : [Tool]
tagline: "Supporting tagline"
tags : [Tool,Linux]
---
{% include JB/setup %}
# ubuntu下配置sublime中文与链接

## 配置命令行启动
- 可以 `vi /usr/bin/subl`, 这样就可以`subl`启动了。

  ```
  #!/bin/sh
  #exec /opt/sublime_text/sublime_text "$@"
  export LD_PRELOAD=/usr/lib/sublime_text_2/libsublime-imfix.so
  exec /usr/lib/sublime_text_2/sublime_text
  ```

## 配置快捷方式
- 新建快捷方式

  ```
  [Desktop Entry]
  Name=Sublime Text 2
  Comment=Sublime_Text_2
  Exec=bash -c "LD_PRELOAD=/usr/lib/sublime_text_2/libsublime-imfix.so exec /usr/lib/sublime_text_2/sublime_text %F"
  Exec=bash -c "LD_PRELOAD=/usr/lib/sublime_text_2/libsublime-imfix.so exec /usr/lib/sublime_text_2/sublime_text -n"
  Exec=bash -c "LD_PRELOAD=/usr/lib/sublime_text_2/libsublime-imfix.so exec /usr/lib/sublime_text_2/sublime_text --command new_file"
  Icon=/usr/lib/sublime_text_2/Icon/48x48/sublime_text.png
  Terminal=false
  Type=Application
  Categories=Application;Development;
  ```
