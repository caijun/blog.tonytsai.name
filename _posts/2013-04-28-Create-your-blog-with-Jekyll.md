---
layout: post
title: Create Your Blog with Jekyll
categories:
- Web
tags:
- Jekyll
- Blog
---

在使用Jekyll搭建个人blog之前，确认你的PC安装有Jekyll。最简单的安装Jekyll方法是通过RubyGems，RubyGems是Ruby包管理框架。在CentOS 6.4上安装RubyGems和Jekyll的步骤如下：

### Install RubyGems
*RubyGems* is a package management framework for *Ruby*

1. Install *Ruby*

```{.bash}
$ sudo yum install ruby
```

2. Install some *Ruby* dependancies
  
```{.bash}
$ sudo yum install ruby-rdoc ruby-devel
```

3. Install *RubyGems*
  
```{.bash}
$ sudo yum install rubygems
```

4. Check *RubyGems* version
  
```{.bash}
$ gem -v
```

5. Update *RubyGems*

```{.bash}
$ sudo gem update --system
```

### Install Jekyll
*Jekyll* is a simple, blog-aware, static site generator in Ruby.

Install *Jekyll* via *RubyGems*

```{.bash}
$ sudo gem install jekyll
```

### 使用Jekyll搭建blog

##### 1. 创建blog主目录
新建一个目录作为blog的主目录

```{.bash}
$ mkdir jekyll_blog
```

##### 2. 创建配置文件
在jekyll_blog目录下新建`_config.yml`文本文件，写入以下内容，其它设置默认。具体设置参见[Jekyll Configuration](https://github.com/mojombo/jekyll/wiki/Configuration)。

```
markdown: pandoc
pandoc: 
    extensions: [mathjax, standalone]
url: http://blog.tonytsai.name
author: Tony Tsai
```

##### 3. 创建模板文件
在jekyll_blog目录下创建`_layouts`目录，用于存放模板文件

```{.bash}
$ mkdir _layouts
```

进入`_layouts`目录，新建`default.html`，加入以下代码，作为blog的默认模板。

```{.html .numberLines}
<!DOCTYPE html>
<html>
  <head>
    <meta http-equiv="content-type" content="text/html; charset=utf-8"/>
    <title>{{ page.title }}</title>
  </head>
        　
  <body>
    {{ content }}
    <footer>
      &copy; Tony Tsai 2013 | All Rights Reserved.
    </footer>
  </body>
</html>
```

Jekyll默认使用[Liquid](https://github.com/shopify/liquid/wiki/liquid-for-designers)标记语言。Liquid提供两种标记语言：`output markup`和`tag markup`，`output markup`用<code>&#123;</code>`{% raw %} { } {% endraw %}`<code>&#125;</code>分割，`tag markup`用<code>&#123;</code>`{% raw %} % % {% endraw %}`<code>&#125;</code>分隔。`page.title`和`content`都是Jekyll提供的[模板数据](https://github.com/mojombo/jekyll/wiki/Template-Data)。

##### 4. 创建文章
在jekyll_blog目录下创建`_posts`目录，用于存放blog文章

```{.bash}
$ mkdir _posts
```

进入`_posts`目录，创建第一篇文章，文件名为`2013-04-28-Create-your-blog-with-Jekyll.md`。（注：文件名必须为"YYYY-MM-DD-Blog-Title.后缀名"，如果采用`html`网页，后缀名为`html`；如果采用[markdown](http://daringfireball.net/projects/markdown/)格式，后缀名为`md`）
在该文件中，写入以下内容：

```{.yaml .numberLines}
---
layout: post
title: Create Your Blog With Jekyll
categories:
- Web
tags:
- Jekyll
- blog
---

...content of this markdown file...
```

每篇文章的头部必须有[YAML Front Matter](https://github.com/mojombo/jekyll/wiki/YAML-Front-Matter)，用来设置一些元数据。它用三根短划线"- - -"标记开始和结束，里面每一行设置一种元数据。"layout: default"表示该文章模板使用`_layouts`目录下的`default.html`文件；"title: Create Your Blog With Jekyll"表示该文章标题是"Create Your Blog With Jekyll"，如果不设置`title`，默认使用文件名作为标题，即"Create your blog with Jekyll"。

##### 5. 创建首页
有了文章之后，还需要一个首页。回到jekyll_blog目录，新建一个`index.md`文件，内容如下：

```{.yaml .numberLines}
---
layout: page
title: Tony Tsai's Blog
---

<ul class="listing">
{% for post in site.posts %}
  {% capture y %}{{post.date | date:"%Y"}}{% endcapture %}
  {% if year != y %}
  {% assign year = y %}
  <li class="listing-seperator">{{ y }}</li>
  {% endif %}
  <li class="listing-item">
    <time datetime="{{ post.date | date:"%Y-%m-%d" }}">{{ post.date | date:"%Y-%m-%d" }}</time>
    <a href="{{ site.url }}{{ post.url }}" title="{{ post.title }}">{{ post.title }}</a>
  </li>
{% endfor %}
</ul>
```

`index.md`参考[谢益辉中文博客](http://yihui.name/cn/)的GitHub[源码](https://github.com/yihui/cn)。首页使用default模板，标题为"Tony Tsai's Blog"。`{% raw %} {% for post in site.posts %} {% endfor %} {% endraw %}`表示对`_posts`目录下的所有文章遍历。<!TODO:更多说明>

最终目录结构为：

```
/jekyll_blog
    |--　_config.yml
    |--　_layouts
    |     |--　default.html
    |--　_posts
    |     |--　2013-04-28-Create-your-blog-with-Jekyll.md
    |--　index.md
```

##### 6. 构建blog网站
构建`blog`网站十分容易，进入`jekyll_blog`目录，运行`jekyll build`即可。

```{.bash}
$ cd jekyll_blog
$ jekyll build
```

可以看到blog网站在新建的`_site`目录中被构建出来。如果直接用浏览器打开`_site/index.html`文件，点击第一篇文章，发现找不到文件。这是因为文章路径采用了相对路径。但Jekyll内置有服务器，只要运行

```{.bash}
$ jekyll server
```

然后在浏览器输入`localhost:4000`或`0.0.0.0:4000`地址就可以浏览blog网站了。

##### 7. 发布blog网站
如果想别人能够通过互联网访问你的blog网站，你需要有域名和具有独立公网IP的服务器。从个人经济能力出发，域名可以考虑购买适合个人使用、相对便宜的域名，比如`.name`，`.me`等；服务器可以考虑购买VPS服务。将`_site`目录上载到虚拟专用服务器 (Virtual Private Server, VPS)，并将blog的域名绑定到`_site`目录。

现在你就拥有自己的个人blog了。