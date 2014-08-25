---
layout: post
title: Introduction to git and github
categories:
- github
tags:
- github
- Mac OS X
---

### git安装
稍后补充。。。

### git 常用命令
1. 将github上面的项目克隆到本地

```{.bash}
$ git clone git@github.com:caijun/blog.tonytsai.name.git
```
2. 查看当前项目下所有文件的状态

```{.bash}
$ git status
```
状态（包括新建、删除、修改）发生改变的文件用红色标出。

3. 将更新后的项目提交到git本地仓库

```{.bash}
$ git add .
```
`.`表示当前项目目录下的所有内容。    
若更新包括删除文件，该命令在`git 2.0`以上版本失效，并产生`warning`提示使用`git add --all`代替。

4. 对更新内容确认并添加确认信息

```{.bash}
$ git commit -m 'commit message'
```
确认信息对于版本控制很关键，每次版本更新时都应该添加描述准确的确认信息。

5. 将项目提到github，首先要与github上面对应的远程仓库建立连接

```{.bash}
$ git remote add origin git@github.com:caijun/blog.tonytsai.name.git
```

6. 查看当前项目远程连接的仓库地址

```{.bash}
$ git remote -v
```

7. 将本地的项目提交到远程仓库

```{.bash}
$ git push -u origin master
```

### git使用常见错误及解决办法
稍后补充。。。

### 资源链接
1. [git/github学习笔记](http://www.cnblogs.com/fnng/archive/2011/08/25/2153807.html)

2. [git/github初级运用自如](http://www.cnblogs.com/fnng/archive/2012/01/07/2315685.html)

