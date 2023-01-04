---
title: Use IPv6 in an ISATAP Way
author: ~
date: '2013-06-10'
categories: ["Windows"]
tags: ["IPv6", "ISATAP", "Windows 7", "THU"]
draft: false
---

地学前沿最后一次课是和宫老师一起吃饭聊天（主要是听宫老师讲），期间宫老师提到了他写的一篇文章[How to find an academic job](http://nature.berkeley.edu/~penggong/Acadjobhunt.pdf)，挂在了他在berkely的个人主页上。回来google了一下，点击下载，结果又出现了恼人的浏览器报错：**无法显示此网页 与auth.ccert.edu.cn 的连接已中断**。google图片、连接THU的IPv6主页和六维空间都会出现该错误。不得不吐槽一下THU的网络，出奇的慢，而且还有流量限制。最近连[Stack Overflow](http://stackoverflow.com/)，[TeX - LaTeX Stack Exchange](http://tex.stackexchange.com/)都不能正常显示了（在BNU都能正常显示），下午甚至WOK都不能完全显示，简直无法工作，而且非常影响工作心情。

是可忍孰不可忍，只能去解决。google了一下，从水木清华上得知是因为紫荆的IPv6挂了，建议通过[ISATAP](http://en.wikipedia.org/wiki/ISATAP)使用IPv6。在Windows 7上的配置过程如下：

1. 以管理员身份运行cmd
2. 在CLI里输入以下命令：

``` bash
> netsh interface ipv6 isatap set router <ISATAP路由器地址>
> netsh interface ipv6 isatap set state enabled
```

THU的ISATAP路由器地址为isatap.tsinghua.edu.cn。

3. 用浏览器去访问[THU IPv6主页](http://ipv6.tsinghua.edu.cn/)，应该可以正常访问了。
4. 如果还是不能正常访问，可能是因为Windows 7系统会在本地LAN中发出IPv6 RA（Router Advertisement，路由器应答），导致相邻用户不走ISATAP，此时最好在本地网络上禁用IPv6协议选项。

现在可以正常访问网络了，再加上[GoAgent](https://code.google.com/p/goagent/)代理，世界又变得美好了。
