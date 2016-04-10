---
layout: post
title: local markdown
category : [Tool]
tagline: "Supporting tagline"
tags : [Tool]
---
{% include JB/setup %}
# 本地编写渲染markdown

## 当前方案：
- 目前本人用atom编写md文件，用atom中的markdown preview预览。但是输出称html，pdf等效果一般。
- 之前使用过sublime 编写预览输出效果都不错。
- 感觉是因为css文件的使用问题。
- 目前安装了pandoc，可以生成html，并可以指定css，但是效果依然不理想。

## 目标
- 希望实现：
  + 文件夹预览，一行命令或一个脚本就可以实现整个文件夹下的markdown预览，或者说
  将所有文件转换成html文件。且不同文件间支持引用。
  + 支持多种引用，除了前面不同文件的引用，还支持图片等引用。
  + 支持导出称pdf。
  + 可以试着使用slide.css，制作幻灯片。
  + 如何实现代码语法高亮
  + 写CV:<https://github.com/elipapa/markdown-cv>
