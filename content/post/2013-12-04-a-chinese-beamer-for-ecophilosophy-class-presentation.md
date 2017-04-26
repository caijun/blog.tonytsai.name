---
title: A Chinese Beamer for Ecophilosophy Class Presentation
author: ~
date: '2013-12-04'
slug: ''
categories: ["LaTeX"]
tags: ["LaTeX", "Beamer", "Chinese"]
draft: yes
---

用Beamer做英文[slides](http://blog.tonytsai.name/2013/06/A-beamer-for-doctoral-english-class-final-presentation/)已经尝试过了，做中文slides一直没有尝试过。借着研究生最后一门课程——生态哲学——课堂展示的机会，做了一个中文[slides](http://tonytsai.name/materials/TonyTsai+20131127.pdf)。这次制作过程中学习了如何添加参考文献、Hyperlinks和跳转Buttons以及脚注。自己一直以来坚持用[JabRef](http://jabref.sourceforge.net/)管理文献，就是为了有一天能用$\LaTeX$写东西时能够方便插入文献。这次折腾了[BibTeX](http://en.wikipedia.org/wiki/BibTeX)，插入文献的确很方便，发现自己的判断还是很明智的。  
使用xeCJK包写中文，因此编译顺序为XeLaTeX + BibTeX + XeLaTeX。其中两次XeLaTeX编译是为了产生中文书签，而BibTeX是为了编译文献。  
猛戳[此处](http://tonytsai.name/materials/TonyTsai+20131127.tex)下载$\TeX$源文件，欢迎研究交流。
