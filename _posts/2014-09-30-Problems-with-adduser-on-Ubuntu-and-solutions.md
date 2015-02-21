---
layout: post
title: Problems with adduser on Ubuntu and solutions
categories:
- Linux
tags:
- Ubuntu
- add user
- shell
- prompt
---

最近想为Ubuntu服务器增加一个用户，然后`ssh`远程登录使用。折腾了好久总算搞定，现将过程中遇到的问题记录下来，以便将来参考。

#### 1. 创建新用户
Linux系统中只有*root*用户有创建其它用户的权限。

切换到*root*用户，创建新用户*zyw* ，并设置密码

```{.bash}
$ su - root
$ useradd zyw
$ passwd zyw
```

#### 2. 用户缺少主目录
然后以*zyw*用户`ssh`登录Ubuntu服务器，却显示以下错误信息：

```
Could not chdir to home directory /home/zyw: No such file or directory
```

很显然问题在于使用`useradd`命令创建用户时，忘了使用`-d`参数指定用户主目录（其实我以为系统会根据用户名自动创建，看来系统没有那么智能）。解决办法是为*zyw*用户创建主目录**/home/zyw**并设定相应权限，输入以下命令：

```{.bash}
$ mkdir /home/zyw
$ chown zyw:zyw /home/zyw
$ chmod 0700 /home/zyw
```
#### 3. 终端提示符不是`[user@host path]$`
再次登录Ubuntu服务器，成功登录。但终端提示符只显示一个`$`，而不是常见的`[user@host path]$`。用`ls`命令列出当前目录清单时，目录和文件也没有用不同颜色标识，而且命令输入错误也不能删除，使用非常不方便。

通过google发现，原因在于用户使用的`shell`不是常用的`bash`。查看当前使用的`shell`，输入：

```{.bash}
$ echo $SHELL
```

果然输出为`sh`，而不是`bash`。解决办法是修改**/etc/passwd**配置文件，将*zyw*用户使用的`shell`改为`bash`。修改**/etc/passwd**需要`sudo`权限，新建的用户使用不了`sudo`，提示该用户不在`sudoers`列表里。解决办法是切换到*root*用户，执行`visudo`或者修改**/etc/sudoers**，在`root    ALL=(ALL)       ALL`下面添加

```
zyw    ALL=(ALL)       ALL
```

这为*zyw*用户添加了所有权限。

切换回*zyw*用户，重新修改**/etc/passwd**，找到

```
zyw:x:1001:1001::/home/zyw:/bin/sh
```

其中最后一个分号后的内容指定*zyw*用户使用的`shell`，将其修改为**/bin/bash**。

#### 4. 终端缺少配置文件
重新登录Terminal，提示符正常，但仍没有颜色高亮。通过`ls -a`列出用户主目录下所有文件（包括隐藏文件），发现缺少终端配置文件**.bashrc**和**.profile**。解决办法是从**/etc/skel**目录下拷贝过来，输入以下命令：

```{.bash}
$ cp /etc/skel/.bashrc /home/zyw/
$ cp /etc/skel/.profile /home/zyw/
```

再次登录Terminal，一切都正常了，可以愉快地玩耍了。

### 参考链接
1. [Could not chdir to home directory: No such file or directory Error and Solution](http://www.cyberciti.biz/faq/linux-unix-appleosx-bsd-could-not-chdir-to-home-directory/)

2. [ubuntu下adduser创建新用户的问题](http://my.oschina.net/shangjx13/blog/32155)

3. [linux 用户账户管理](http://my.oschina.net/shangjx13/blog/30626)

4. [Why does my Linux prompt show a $, instead of the login name and path?](http://superuser.com/questions/68397/why-does-my-linux-prompt-show-a-instead-of-the-login-name-and-path)
