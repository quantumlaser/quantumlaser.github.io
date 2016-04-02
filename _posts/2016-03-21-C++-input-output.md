---
layout: post
title: C++ 输入输出总结
category : C++
tagline: "Supporting tagline"
tags : [C++]
---
{% include JB/setup %}
# C++ 输入输出总结
## Reference:
- [Link](http://www.cnblogs.com/wanghao111/archive/2009/09/05/1560822.html)
- http://www.cnblogs.com/A-Song/archive/2012/01/29/2331204.html
## cin
- cin可以抛弃开头的终止符，但不会丢弃终止符
## cin.get()
- 接受一个字符`c = cin.get(); ` or ` cin.get(c);`。可以接受空格、回车。终止符不丢弃
- `cin.get(字符数组名,接收字符数目,终止符)` 用来接收一行字符串,可以接收空格。 默认enter终止。终止符不丢弃。
  + 用cin.get()读一行，会将enter留在流中，下次用cin.get()将会读取enter导致失败。通过
  `cin.get(a,20).get()`可以避免这个影响，但是最好用cin.getline()
## cin.getline()
  + 与`cin.get`基本一样。默认终止符enter，但是会丢弃。如果终止符不是enter，enter也可以都进去。
## cin.ignore(n,c)
- 跳过N个字符，或者遇到c停止。c丢弃
- `cin.ignore` 为 `cin.ignore(1,EOF)`
## getline,gets,getchar()
- 需要配合`string`使用
- `getline(cin, str, 终止符)`读取一行到string，到enter或终止符
