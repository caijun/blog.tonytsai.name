---
layout: post
title: How to Connect to VMware Machine (CentOS 6.4) via SSH
categories: 
- Linux
tags: 
- SSH
- VMWare Machine
- CentOS
---

之前使用CentOS虚拟机都是在GUI里操作，四年前配置的笔记本实在吃不消，非常的卡，都没有了工作的欲望。最近在做《高性能计算导论》课程作业时，需要SSH远程登录Linux环境操作，就想到是否也可以SSH远程登录我的CentOS虚拟机，在Terminal里操作，节省一些资源。Google了一下，发现的确可以，现将配置过程记录下来。（昨天上完《高性能计算导论》课程和付老师交流，他说不要怕花时间在配置工具上，我笑了。）

### 1. 设置虚拟机网络连接为NAT
打开CentOS虚拟机的Virtual Machine Settings，点击Network Adapter，将Network connection设置为NAT。我的CentOS虚拟机一直都是通过NAT方式联网的。

![设置联网模式为NAT](http://tonytsai.name/materials/NAT.png)

### 2. 查看主机和虚拟机的IP并测试ping
在CentOS中查看IP只需在终端中输入

```{.bash}
$ ifconfig
$ ping ipaddr
```
在WIN 7查看IP只需在CLI中输入

```
> ipconfig
> ping ipaddr
```
在CentOS中ping 主机WIN 7的IP，在WIN 7中ping虚拟机CentOS的IP，保证能够互相ping通。

### 3. 更改openssh配置文件
CentOS 6.4默认已经安装openssh，更改openssh配置文件`/etc/ssh/sshd_config`，去掉

```
Port 22
ListenAddress 0.0.0.0
Protocol 2
```
前面的#。

### 4. 开启ssh服务
在CentOS中开启sshd服务只需输入

```{.bash}
$ service sshd start
```
设置sshd服务开机自启动，输入

```{.bash}
$ chkconfig sshd on
```
查看sshd服务是否开启，输入

```{.bash}
$ ps -le|grep sshd
```
显示sshd信息说明服务已经开启。

### 5. 使用ssh连接工具连接
ssh连接工具推荐使用[NetSarang Xmanager](http://www.netsarang.com/products/xme_overview.html)，它是一款商业软件，能够较好支持中文，集FTP、SSH FTP和图形远程登录于一身，非常方便，没有安装多个软件的麻烦。[Putty](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)非常小巧，而且开源，也能够较好支持中文，但是SSH文件传输需要另外使用PSFTP程序，也推荐使用。

连接时Host填写CentOS的IP，端口号默认22，用户名和密码使用CentOS的登录名和密码即可。
