---
layout: post
title: C++ container
category : C++
tagline: "Supporting tagline"
tags : [C++]
---
{% include JB/setup %}
# C++ 容器
## string
- Reference
  + [http://www.cnblogs.com/xFreedom/archive/2011/05/16/2048037.html](http://www.cnblogs.com/xFreedom/archive/2011/05/16/2048037.html)
  + [http://www.cplusplus.com/reference/string/string/operator%3E%3E/](http://www.cplusplus.com/reference/string/string/operator%3E%3E/)
- IO
  + `cin>>str` 遇空格回车停止
  + `getline(cin, str)` 读一行，包括空格，遇到回车、终止符停止。
- Usage
  + 去除所有空格`str.erase(remove_if(str.begin(), str.end(), isspace), str.end());`
