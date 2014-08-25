---
title: Links
layout: page
comments: yes
---

### People of Interest
* [Hadley Wickham](http://had.co.nz/)
    + [R packages](http://r-pkgs.had.co.nz/)
    + [Advanced R](http://adv-r.had.co.nz/)
* [Yihui Xie](http://yihui.name/)

### Softwares
* [R](http://www.r-project.org/) 自由的统计计算和图形软件环境，有丰富的package可以使用。习惯使用R的集成开发环境——[RStudio](hhtps://www/rstudio.com/)写R代码。自己开发的R package：
    + [geoChina](http://cran.r-project.org/web/packages/geoChina/index.html)：(反向)地理编码以及WGS-84，GCJ-02和BD-09坐标的相互转化。由于`geoChina`部分算法可能与国内部分测绘法规相违背，因此`geoChina`已从`CRAN`上撤掉。但`geoChina`的开发维护没有停止，仍可从github上获取最新的源码。
* [Python](https://www.python.org/) 有丰富的package可以使用，能够轻松快速地完成很多常见任务。目前主要编写网络程序爬取数据（推荐[Beautiful Soup](http://www.crummy.com/software/BeautifulSoup/)和[selenium](https://pypi.python.org/pypi/selenium) package）以及通过[ArcPy](http://resources.arcgis.com/en/help/main/10.1/index.html#//000v000000v7000000) package调用ArcGIS中的GIS函数，完成一些地理数据处理和地图制图的批处理任务。自己编写的Python脚本：
    + [PFD](https://github.com/caijun/PFD)：抓取网络数据
* [ArcGIS](https://www.arcgis.com/) GIS专业软件，主要用于空间矢量数据数据处理、空间分析以及地图制图。利用ArcPy可以扩展ArcGIS的功能。    
* [ENVI/IDL](http://www.exelisvis.com/) RS专业软件，ENVI是[IDL](http://en.wikipedia.org/wiki/IDL_(programming_language))编程语言开发的遥感影像处理软件平台。利用IDL可以扩展ENVI的功能。自己编写的IDL程序：
    + [LCF](https://github.com/caijun/LCF)：从MOD/MYD35云掩膜产品探测低云并计算低云频率，涉及常用的遥感影像批处理操作——几何校正、重采样、镶嵌、裁剪和ROI提取等。
