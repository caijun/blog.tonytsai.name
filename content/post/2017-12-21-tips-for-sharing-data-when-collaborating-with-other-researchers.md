---
title: Tips for Sharing Data When Collaborating with Other Researchers
author: ~
date: '2017-12-21'
slug: tips-for-sharing-data-when-collaborating-with-other-researchers
categories: ['Research']
tags: ['data sharing', 'collaboration', 'encoding']
---

Recently I have been collaborating with others on several research projects. In the process of collaboration, data exchanging is inevitable; however, for collaborators lack of rich experience in data analysis and computer skills, the way that they are sharing the data results in inefficient collaboration. Therefore, I summarize following tips for sharing data when collorating with other researchers.

* First of all, please include a README file describing the data, including file contents, variable names, value formats, units etc. Without README, nobody knows what the data is.
* Try to store data in plain text files such as [csv](https://en.wikipedia.org/wiki/Comma-separated_values), txt. Therefore, other researchers can open the data files by a text editor (which is shipped with every OS.) and don't need to install professional softwares. Sharing data in csv files is prefered. The following are what you need to pay attention to when storing data in csv files.
    - Include the header, otherwise no one knows what the data is.
    - Quote the value if comma is inside a cell as comma is default used as the separator in csv. 
    - Encode csv to [UTF-8](https://en.wikipedia.org/wiki/UTF-8). For Windows OS with simplified Chinese, csv files are usually encoded to [cp936](https://en.wikipedia.org/wiki/Code_page_1386). For other encoding please state it in README. For Excel users on Windows OS with simplified Chinese, the correct way to open a UTF-8 encoded csv file is given [here](https://www.itg.ias.edu/content/how-import-csv-file-uses-utf-8-character-encoding-0). If you directly double click on it, you will see messy characters.
* Try to avoid storing information in file names. The better way is to store them as variables in csv files.
* For date variable, store it in the commom format such as YYYY-MM-DD or YYYY/MM/DD. By such format, the year, month and day information can be easily extracted when necessary while analyzing the data. The bad example is that the year information included in filenames while the month and day information stored in file contents. The same rule applies to datetime variable.
* Try to store the whole data in a single csv file when the data size is not huge.
* Don't change the way data is organised. When you update the data, try to store data as the way it is organised before. Because your collaborators may use your data as input. Any change you made in the way for organising the data may result in others changing their analysis code.

This post will keep updating and more tips will be added as my collaboration experience increases.