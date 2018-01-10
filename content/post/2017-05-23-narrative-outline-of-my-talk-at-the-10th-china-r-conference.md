---
title: Narrative Outline of My Talk at the 10th China R Conference
author: ~
date: '2017-05-23'
slug: narrative-outline-of-my-talk-at-the-10th-china-r-conference
categories: ['R']
tags: ["China-R Conference", "Epidemics", "Influenza"]
draft: yes
---

The [10th China R Conference](http://china-r.org/bj2017/) was held in Tsinghua University during May 19 - 21. I initialized the session of *R in Public Health*. Though it was my first time to organize a session, to invite speakers, and to host it, the session was successful. The number of audiences was more than 55 persons and beyond my expectation, and the discussions were enthusiastic. I believe that via my talk more Chinese have known the [R Epidemics Consortium (RECON)](http://www.repidemicsconsortium.org/) and they may try to use RECON packages to facilitate their epidemics research.

Since materials on RECON of [my slides](https://github.com/caijun/talks/blob/master/china-r_2017/china-r_2017.pdf) were partly provided by [Dr. Thibaut Jombart](https://sites.google.com/site/thibautjombart/), the founder of RECON, I had sent my slides and narrative outline to Dr. Jombart for reviewing before my talk. The narrative outline of my talk is as follows:

"On May 21, 2017 I will give a talk on **R Epidemics Consortium and Using Its Packages to Analyze Influenza Data** (see slide 1) during the *R in Public Health* session of the 10th China R conference. The talk consists of four parts (see slide 2).

During the talk, I will firstly introduce the precursor of RECON — Hackout 3 (see slide 3), then RECON (see slides 4 and 5) and its forum (see slide 6). Of course, I will call on audiences to join RECON.

In the second part of RECON packages, I will introduce the aims of RECON packages (see slide 7) and then list all packages that RECON has (see slides 8 and 9), among which I will focus on three packages — *outbreaks*, *incidence* and *EpiEstim* as I will use them to demostrate how to use RECON packages to analyze influenza data in the following parts. 

The slides of first two parts mainly refer to Dr. Jombart's talk - [Webinar_2017](https://github.com/thibautjombart/talks/tree/master/Webinar_2017) and I will give thanks to Dr. Jombart during the talk.

In the Epidemic Curve and incidence package part, I will illustrate the concept of Epidemic Curve (see slide 9) and show that the ISOweek-based weekly stacked bar plot is ubiquitous plot in epidemiological reports by such a figure of China Influenza Laboratory Surveillance Information from WHO GISRS (see slide 10). In the following slides 12 - 14, I will demostrate how **tedious** to plot daily, ISOweek-based epidemic curve of influenza A H7N9 in China, 2013 with group of gender. The dataset used for plots is from *outbreaks* package, which will be firstly introduced in slide 12. Then *incidence* package will go to the stage (see slide 15) and I will demotrate how **easy** to plot those figures using *incidence* package and emphasize on the feature of ISOweek support that I contributed to *incidence* package (see slides 16 - 18).

In the last part of Time-varying Reproduction Number $R_t$ and *EpiEstim* package, I will first introduce the concept of reproduction number R and its significant implication (see slide 19) and illustrate how it relates to epidemic curve (see slide 20). Then I will introduce *EpiEstim* package developed by [Dr. Anne Cori](http://www.imperial.ac.uk/people/a.cori) and the paper behind *EpiEstim* package (see slide 21). In the last R code demo (see slides 22 and 23), I will use *EpiEstim* package to estimate the instantaneous reproduction number for laboratory confirmed pandemic 2009 influenza A (H1N1) in Beijing and plot the estimation with holidays overlaying to explore the holidays effect on the transmission of pandemic 2009 influenza A (H1N1). You will see in the demo *incidence* package will also be used. By using *EpiEstim* package to analyze my own pandemic 2009 influenza A (H1N1) in China, I found a [bug](https://github.com/annecori/EpiEstim/issues/4) of *EpiEstim* package and reported it to Dr. Cori. This is why I would like to introduce *EpiEstim* package during this talk."