---
layout: post
title: Commonly used ArcGIS tools by myself
categories:
- GIS
tags:
- ArcGIS toolbox
- spatial data processing
---
  
ArcGIS toolbox提供了丰富的工具，但只有一些是在平常的空间数据处理过程中自己会用到。即使如此，每次自己进行一个处理操作时都要再去google一遍（原谅自己的脑子越来越不好使），存在太多的重复性劳动，十分浪费时间。因此十分有必要将自己常面对的数据处理需求及相应的工具实现记录下来。

#### 1. 矢量数据裁剪

```
Analysis Tools -> Extract -> Clip
```
#### 2. 栅格数据裁剪

```
Spatial Analyst -> Extraction -> Extract by Mask
```
#### 3. 提取多边形外边界
首先将多边形转化为多线

```
Data Management Tools -> Features -> Polygon To Line
```
然后删除属性表里`LEFT_FID != -1`的feature。

#### 4. 定义投影

```
Data Management Tools -> Projections and Transformations -> Define Projection
```
#### 5. Merge multiple polygons into multipart feature

```
Data Mangement Tools -> Generalization -> Dissolve
```

#### 6. Overlay analysis (count, attribute statistics) for points in polygon

```
Analysis Tools -> Overlay -> Spatial Join
```

<br>
其实知道一个处理操作的关键词（比如clip，projection等），可以直接在ArcGIS的`Search`窗口搜索工具，一般都能找到自己想要的工具。
