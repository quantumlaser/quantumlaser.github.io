---
layout: post
title: 搭建Github Page个人博客
category : Web
tagline: "Supporting tagline"
tags : [Jekyll, Web, Markdown]
---
{% include JB/setup %}
# 利用Github page + jekyll + markdown 打造个人博客
---

## Reference
1. [Github Page 首页](https://pages.github.com/])
2. [官方使用jekyll搭建博客教程](https://help.github.com/articles/using-jekyll-as-a-static-site-generator-with-github-pages/)
3. Fork了这个[模板](http://enml.github.io/site/)
  + [Github地址：enml/blog](https://github.com/enml/blog/tree/jekyll-blog)

## 说明
- [kmarkdown 语法](http://platinhom.github.io/2015/11/06/Kramdown-note/)
  - 其中直接连接用`<http://baidu.com>`
- 使用rouge进行代码着色，除了更改_config.yml外，还需要在`/_includes/themes/default.html`
里添加css。本博客css位置为：`/assets/rouge/rouge.css`。这个css是可以手动指定的。
- 表格没有框线，是因为没有定义css。


## Usage
- 本地开启: `jekyll server -w`

## Tips
- css失效：直接Fork后推到本人的github page上，css无效，后来发现需要在[_config.yml](https://github.com/quantumlaser/quantumlaser.github.io/blob/master/_config.yml)文件中更改**production_url**，注释掉**BASE_PATH**，因为本人配置了DNS解析和CNAME。
- 上一步解决了首页的css失效问题，但是对于具体的blog的css依然失效，最后发现所有的css指向了:[default.html](https://github.com/quantumlaser/quantumlaser.github.io/blob/master/_includes/themes/twitter/default.html)。通过手动更改`/_includes/themes/default.html`中18-21行的css路径，终于解决了css问题。原作者此文件<https://github.com/enml/blog/blob/jekyll-blog/_includes/themes/twitter/default.html>

## TODO
- 官方关于jekyll 3.0 升级说明：        
   <https://github.com/blog/2100-github-pages-now-faster-and-simpler-with-jekyll-3-0>
- 尝试更改一些css，比如改改颜色啊
- 尝试建立一个http服务，提供点击下载文件功能
- 提供文章搜索功能，内嵌谷歌搜索？
- about me 是否可以用md编写，然后跳转到该md渲染的html文件
