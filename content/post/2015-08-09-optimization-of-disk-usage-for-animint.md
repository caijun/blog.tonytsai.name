---
title: Optimization of Disk Usage for animint
author: ~
date: '2015-08-09'
categories: ["R"]
tags: ["animint", "optimization", "disk space", "GSoC 2015"]
draft: false
---

## Problem
The easy test for becoming a potential student of [animint project](https://github.com/rstats-gsoc/gsoc2015/wiki/Animint) for GSoC 2015 is to use [animint](https://github.com/tdhock/animint) to visualize some data from your domain of expertise, and upload your visualization to the web using `animint2gist`. I used animint to visualize the data from the [CDC's State-level FluView](http://gis.cdc.gov/grasp/fluview/main.html), which is a main data source of my Ph.D. [influenza research](https://github.com/caijun/res4flu#databases-for-influenza-research). The script for generating the FluView viz can be found in [AnimintTest](https://github.com/caijun/AnimintTest) repository.

As shown in following figure, the FluView viz is comprised of two ggplots: the top is a heatmap, and the bottom is a map of US lower 48 states. In the top heatmap, `WEEKEND` is a selection variable. As the selected `WEEKEND` changes, the bottom state map is re-drawn to show the ILI activity levels across US lower 48 states at the selected `WEEKEND`, whose colors are mapping to the ILI activity levels.

![Screenshot of FluView viz](http://tonytsai.name/materials/FluView.png)

However, there are some problems when using the current animint package to produce above FluView viz. First, after making the FluView viz, it took long time for `animint2gist` to upload the viz to [bl.ocks.org](http://bl.ocks.org), but still failed.
``` r
> system.time(animint2gist(viz, out.dir = "FluView-old"))
Loading required namespace: gistr
Error in res$errors : object of type 'externalptr' is not subsettable
Timing stopped at: 148.793 16.205 412.56
```

So I had to upload the FluView viz to [my personal website](http://tonytsai.name/FluView-old/index.html) to demonstrate it to [Toby Hocking](https://github.com/tdhock), the creator of animint package. Second, when you play with the FluView viz, you will feel strongly that the interactivity experience of FluView viz is less responsive. After clicking on the top heatmap,  it took long time for the viz to update the bottom map to show the ILI activity levels at selected `WEEKEND`.

Actually, these problems are caused by the large filesize of chunks generated by animint. Using the current animint package, the FluView viz took about **219** MB of chunks, which was too large for `animint2gist` to upload it. Individual chunk of `geom4_polygon_stateMap` for each `WEEKEND` had a filesize of **708** KB, which also took a while for the browser to load it and made the viz less responsive.
``` bash
$ ls FluView-old/*.tsv|wc -l
     316
$ du -hsc FluView-old/*.tsv|tail
708K	FluView-old/geom4_polygon_stateMap_chunk91.tsv
708K	FluView-old/geom4_polygon_stateMap_chunk92.tsv
708K	FluView-old/geom4_polygon_stateMap_chunk93.tsv
708K	FluView-old/geom4_polygon_stateMap_chunk94.tsv
708K	FluView-old/geom4_polygon_stateMap_chunk95.tsv
708K	FluView-old/geom4_polygon_stateMap_chunk96.tsv
708K	FluView-old/geom4_polygon_stateMap_chunk97.tsv
708K	FluView-old/geom4_polygon_stateMap_chunk98.tsv
708K	FluView-old/geom4_polygon_stateMap_chunk99.tsv
219M	total
```

Therefore, the disk usage of current animint package need to be optimized, at least for the FluView viz. As stated in the OPTIMIZATION category from the **TODO list of features to implement** in the [README](https://github.com/tdhock/animint/blob/master/README.md) file of animint package, optimizations in terms of compiler/render speed, disk usage, memory, etc make animint easier to visualize large data sets, for example, the CDC FluView dataset. Hence, optimizations for animint become key project goals for my animint [project proposal](https://github.com/caijun/AnimintTest/blob/master/gsoc-r-2015-animint-proposal.md) for GSoC-R 2015.

## Solution
By inspecting the chunk files of FluView viz, we can see that each chunk of `geom4_polygon_stateMap` has saved the coordinates and fill colours of state polygons. Since using ggplot2 to draw maps, spatial objects are stored in data.frame, in which each row consists of **coordinates** of a vertex and **attributes** of the spatial object that the vertex belongs to (e.g. fill colours of state polygons in the FluView viz); however, in the FluView viz, the number of vertex (11527) to draw the borders of state polygons are far more than the number of state polygons (62 other than 48 because holes in some polygons), which leads to the repeated storage of attributes. By virtue of my background of GIS (Geographic Information System), I know the best spatial data storing approach is like [shapefile](https://en.wikipedia.org/wiki/Shapefile), one advantage of which is to separately store the shape and attribute in .shp file and .dbf table, avoiding the repeated storage of shape coordinates and simplying the storage of object attributes. So for spatial objects such as the FluView viz, I proposed the new chunk storing strategy as shown in the topleft panel of following figure.

By discussing with Toby Hocking, for non-spatial objects such as the frequent used [WorldBank viz](http://bl.ocks.org/caijun/raw/c7899e4c614d0fe37423/), there are also redundant information (common columns) across chunks. Hence, the common columns and varied columns should also be saved separately to chunk files. A new chunk storing strategy for non-spatial objects shown in topright panel of following figure is proposed.

{{< imgcap title="New chunk storing strategy" src="http://tonytsai.name/materials/animint.png" >}}

For generality, the optimization of disk uage for animint can be abstracted into the new chunk storing strategy shown in the bottom panel of above figure. The columns of each chunk are divided into three categories: **common** columns across chunks, **varied** columns across chunks, and **key** columns. The common and key columns are saved into an **common** chunk, which is an extra added chunk; the varied and key columns are saved to **varied** chunks, the number of which are the same. Key columns are saved to both common chunk and varied chunks to ensure that the renderer can recover the original chunk through joining common chunk to varied chunks by key columns. The key columns are expressed as `union(group, nest_order)`, of which `group` is the column name that represents the row index before splitting the original chunk, and `nest_order` is a variable that is used for grouping rows in (both common and varied) chunks into a hierarchical tree structure in the renderer. It's notable that `group` has already been included in the `nest_order` variable for some visualizations.

Therefore, the implements of the optimization for animint can be simplified into three main parts:

* Determining the common columns across chunks in the compiler
* Formalising the key columns in the compiler
* Recovering original chunks through joining common chunk into varied chunks by key columns in the renderer

To further reduce writing time and disk space, any rows with NA from varied chunks are not saved to tsv files. A benefit of deleting NA values in the compiler is to fix the warning of **Unexpected value NaN parsing attributes** from D3.js in the renderer.

## Improvement

The pull request of [optimizations to save disk space](https://github.com/tdhock/animint/pull/76) implements the new version of animint compiler and renderer based on the new chunk storing strategy.

For the above FluView viz, though an extra common chunk is generated by the new animint compiler, the viz takes only **4.4** MB, which is almost $1/50$ of the original filesize produced by the old animint compiler. Individual varied chunk of `geom4_polygon_stateMap` for each `WEEKEND` has a filesize of only **4** KB, which is almost $1/180$ of the original filesize. The sharp descrease in varied chunk filesize  definitely reduces the loading time for the browser and makes the viz responsive.
``` bash
$ ls FluView-new/*.tsv|wc -l
     317
$ du -hsc FluView-new/*.tsv|tail
4.0K	FluView-new/geom4_polygon_stateMap_chunk92.tsv
4.0K	FluView-new/geom4_polygon_stateMap_chunk93.tsv
4.0K	FluView-new/geom4_polygon_stateMap_chunk94.tsv
4.0K	FluView-new/geom4_polygon_stateMap_chunk95.tsv
4.0K	FluView-new/geom4_polygon_stateMap_chunk96.tsv
4.0K	FluView-new/geom4_polygon_stateMap_chunk97.tsv
4.0K	FluView-new/geom4_polygon_stateMap_chunk98.tsv
4.0K	FluView-new/geom4_polygon_stateMap_chunk99.tsv
588K	FluView-new/geom4_polygon_stateMap_chunk_common.tsv
4.4M	total
```

The save chunk process of new animint compiler becomes more complex than the old one, but there are less chunk contents to write onto disk. As a result of trade-off, the compiling speed of the new animint compiler doesn't increase dramatically comparing to the old one.

```r
> system.time(animint2dir(viz, out.dir = "FluView-old"))
   user  system elapsed 
115.830  12.223 128.720
```

```r
> system.time(animint2dir(viz, out.dir = "FluView-new"))
   user  system elapsed 
115.347  13.833 130.460 
```

Now we can successfully post the FluView viz on gist by `animint2gist`, which takes almost the same amount of time with `animint2dir`. Playing with the [result](http://bl.ocks.org/caijun/raw/7ff9b0c53f78d0491366/), you can obviously feel that the interactivity experience has been improved. After clicking on the top heatmap, the bottom state map is updated in time that we can bear.
``` r
> system.time(animint2gist(viz, out.dir = "FluView-new"))
   user  system elapsed 
 98.872  17.935 134.384 
```

For non-spatial objects, the new animint compiler can also reduce disk usage. For example the [WorldBank viz](http://bl.ocks.org/caijun/raw/c7899e4c614d0fe37423/), the disk space of chunk tsvs is reduced from **3.6** MB to **2.8** MB.
``` bash
$ du -hsc WorldBank-old/*.tsv|tail
 16K	WorldBank-old/geom2_text_scatter_chunk52.tsv
8.0K	WorldBank-old/geom2_text_scatter_chunk53.tsv
 16K	WorldBank-old/geom2_text_scatter_chunk6.tsv
 16K	WorldBank-old/geom2_text_scatter_chunk7.tsv
 16K	WorldBank-old/geom2_text_scatter_chunk8.tsv
 16K	WorldBank-old/geom2_text_scatter_chunk9.tsv
4.0K	WorldBank-old/geom3_text_scatter_chunk1.tsv
4.0K	WorldBank-old/geom4_tallrect_ts_chunk1.tsv
1.0M	WorldBank-old/geom5_line_ts_chunk1.tsv
3.6M	total

$ du -hsc WorldBank-new/*.tsv|tail
 12K	WorldBank-new/geom2_text_scatter_chunk52.tsv
 12K	WorldBank-new/geom2_text_scatter_chunk6.tsv
 12K	WorldBank-new/geom2_text_scatter_chunk7.tsv
 12K	WorldBank-new/geom2_text_scatter_chunk8.tsv
 12K	WorldBank-new/geom2_text_scatter_chunk9.tsv
8.0K	WorldBank-new/geom2_text_scatter_chunk_common.tsv
4.0K	WorldBank-new/geom3_text_scatter_chunk1.tsv
4.0K	WorldBank-new/geom4_tallrect_ts_chunk1.tsv
948K	WorldBank-new/geom5_line_ts_chunk1.tsv
2.8M	total
```

## Future Work
For small dataset like WorldBank viz, the animation works well for the new animint compiler and renderer; however, adding `time = list(variable = "WEEKEND", ms = 3000), duration = list(WEEKEND = 1000)` to the FluView viz, the new renderer still doesn't provide smooth transitions between animation frames at different `WEENEND` with varied chunks small enough. This is because how the renderer draws state map hasn't been optimized for the new chunk storing strategy. Actually, comparing to the old one, an extra step of joining common chunk into all varied chunks by key columns is added to the new renderer during chunk downloading process. 

For each animation frame, the polygons of state map are constant and only the fill colours for polygons change. Hence, the best rendering approach is to separate geom drawing and attribute rendering. The renderer draws polygons of state map only at the initial rendering step using the common chunk, and for next animation frames only the fill colours for polygons are re-drawn only using corresponding varied chunks. 
