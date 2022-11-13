---
layout: post
title: Note on outliers in statistics [In writing]
---

Knowledge on statistics is more important than any when it comes to the development of [Number Crunching Motion](https://github.com/Akatmks/Number-Crunching-Motion). This note lists all the possible ways Akatsumekusa found to remove outliers from a sample, and then explains Akatsumekusa's choice in the case of Number Crunching AAE Export.  

### Z-score

The Z-score of an observation is defined as  
$$Z_i = \frac{x_i - \bar{x}}{s}$$
with $\bar{x}$ and $s$ denoting the sample mean and sample standard deviation respectively. Data with a Z-score with an absolute value of greater than 3 could be labeled as potential outliers.  

### Iglewicz and Hoaglin's modified Z-score

The Z-score of an observation is defined as  
$$M_i = \frac{0.6745(x_i - \tilde{x})}{\mathit{MAD}}$$
with $\tilde{x}$ denoting the sample median and $\mathit{MAD}$ representing the median absolute deviation. Iglewicz and Hoaglin recommends that modified Z-scores with an absolute value of greater than 3.5 be labeled as potential outliers.  

### Interquartile range

Data that falls below $Q_1 - 1.5\mathit{IQR}$ or above $Q_3 + 1.5\mathit{IQR}$ could be labeled as potential outliers, with $Q_1$ and $Q_3$ denoting 25<sup>th</sup> and 75<sup>th</sup> percentile of the sample respectively, and $\mathit{IQR}$ representing the interquartile range.  

### Various sources

These sites helps Akatsumekusa tremendously when researching the topic.

* [NIST/SEMATECH Engineering Statistics Handbook](https://www.itl.nist.gov/div898/handbook/eda/section3/eda35h.htm)  
* [Wikipedia](https://en.wikipedia.org/wiki/Statistics)  

Some contents in this note are directly copied from the following sites.  

* Z-score, Iglewicz and Hoaglin's modified Z-score: [NIST/SEMATECH Engineering Statistics Handbook](https://www.itl.nist.gov/div898/handbook/eda/section3/eda35h.htm) ([„Public information“](https://www.nist.gov/oism/copyrights))  