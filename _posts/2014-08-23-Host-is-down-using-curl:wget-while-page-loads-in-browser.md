---
layout: post
title: Host Is Down Using curl/wget While Page Loads in Browser
categories:
- Mac OS X
tags:
- GoAgent
- Proxy
- geoChina
---
  
### 问题描述
最近想更新自己开发的[*geoChina*](https://github.com/caijun/geoChina)，发现在Mac上运行以下代码

```{.r}
library(geoChina)
geocode('Tsinghua University', api = 'google', ocs = 'GCJ-02', messaging = T)
```
出错，返回

```
calling http://maps.googleapis.com/maps/api/geocode/json?address=Tsinghua%20University&sensor=false ...
Warning message:
In readLines(connect, warn = FALSE) :
  unable to connect to 'maps.googleapis.com' on port 80.
Error in fromJSON(paste(readLines(connect, warn = FALSE), collapse = "")) : 
  error in evaluating the argument 'content' in selecting a method for function 'fromJSON': Error in readLines(connect, warn = FALSE) : cannot open the connection
```
将`geocode`封装的调用直接在浏览器里打开，能返回正确的json结果。而且该函数在Windows上运行正常。

### 问题分析
*geoChina*的开发的确是在Mac上完成的，但代码并没有针对不同的操作系统作了不同处理。因此可以初步判断是Mac操作系统的终端网络通讯出现了问题。http服务默认80端口，https服务默认443端口，从报错信息来看，终端网络通讯的80端口出现了问题。

`curl`或`wget`命令能够实现终端网页数据内容下载，因此可以用来验证初步判断。在终端输入

```{.bash}
$ wget www.google.com
```
返回

```
Resolving www.google.com... 2404:6800:4005:805::1012, 173.194.127.244, 173.194.127.242, ...
Connecting to www.google.com|2404:6800:4005:805::1012|:80... failed: Host is down.
```
输入

```{.bash}
$ curl -v -L www.google.com
```
返回

```
* Rebuilt URL to: www.google.com/
* Hostname was NOT found in DNS cache
*   Trying 2404:6800:4005:801::1010...
*   Trying 173.194.127.241...
* connect to 2404:6800:4005:801::1010 port 80 failed: Host is down
* connect to 173.194.127.241 port 80 failed: Host is down
```
都显示连接失败，`Host is down`。而将访问的网站换成国内网站，比如`www.baidu.com`，则都能访问成功，下载到网站首页。

这说明终端网络通讯80端口出问题的只是google等国外易干扰服务，而不是国内网络服务。自己Mac上的应用程序有此影响的只能是代理服务了。经过一番google，发现有人遇到和我十分类似的问题[[1]](#[1])，回答都是代理作祟，基本可以肯定是自己使用*GoAgent*代理导致终端无法通过80端口访问google服务。

### 问题解决
[[1]](#[1])给出的解决方案是在终端设置http/https代理，google Mac OS X终端设置代理的方法，找到[[2]](#[2])。只需清楚*GoAgent*代理服务端口，然后编辑`~/.bash_profile`文件，输入

```{.bash}
export http_proxy='http://localhost:8103'
export https_proxy='http://localhost:8103'
```
保存`.bash_profile`文件，在终端更新`.bash_profile`生效

```{.bash}
$ source .bash_profile
```
现在`curl`或`wget`命令都可以通过代理访问google服务了，但Mac上`geoChina::geocode`运行还是时好时坏，看来`geoChina`的开发维护只能暂时转到Windows上了。

如有同侪知道解决办法，烦请邮件告知。

<br>
PS:本文采用了[添加文内注释/参考文献引用](http://celavie.me/lib/2013/10/19/Markdown%E5%86%99%E4%BD%9C%EF%BC%88%E4%BA%8C%EF%BC%89.html)*Markdown*写法。

### 参考链接
<span id="[1]">[1]</span>: [Unable to resolve host using curl/wget while page loads in browser](http://superuser.com/questions/518297/unable-to-resolve-host-using-curl-wget-while-page-loads-in-browser)

<span id="[2]">[2]</span>: [如何为MacOS X终端设置代理](http://codelife.me/blog/2012/09/02/how-to-set-proxy-for-terminal/)

### 更新
由于GFW的原因，Google服务在国内的使用受影响。在国内调用Google Maps API时推荐使用ditu.google.cn，而不是maps.google.com。解决思路是通过调用GeoIP服务，查询用户是在国内还是国外，指定不同的Google Maps API调用链接。[*geoChina*](https://github.com/caijun/geoChina)已更新至`1.1.3`。