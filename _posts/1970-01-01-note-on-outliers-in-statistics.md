---
layout: post
title: Note on outliers in statistics [In writing]
---

Knowledge on statistics is more important than any when it comes to the development of [Number Crunching Motion](https://github.com/Akatmks/Number-Crunching-Motion). This note lists all the possible methods Akatsumekusa found to remove outliers from a sample, and explains Akatsumekusa's choice in the case of Number Crunching AAE Export.  

## Detection of outliers

### Z-score

The Z-score of an observation is defined as  

$$Z_i = \frac{x_i - \bar{x}}{s}$$

with $\bar{x}$ and $s$ denoting the sample mean and sample standard deviation respectively. Data with a Z-score with an absolute value of greater than $3$ could be labeled as potential outliers.  
Note that Z-score could be misleading as the maximum Z-score for an observation is at most $(N - 1) / \sqrt{N}$

### Iglewicz and Hoaglin's modified Z-score

The Z-score of an observation is defined as  

$$M_i = \frac{0.6745(x_i - \tilde{x})}{\mathit{MAD}}$$

with $\tilde{x}$ denoting the sample median and $\mathit{MAD}$ representing the median absolute deviation. Iglewicz and Hoaglin recommends that modified Z-scores with an absolute value of greater than $3.5$ be labeled as potential outliers.  

### Interquartile range

Data that falls below $Q_1 - 1.5\mathit{IQR}$ or above $Q_3 + 1.5\mathit{IQR}$ could be labeled as potential outliers, with $Q_1$ and $Q_3$ denoting 25<sup>th</sup> and 75<sup>th</sup> percentile of the sample respectively, and $\mathit{IQR}$ representing the interquartile range.  

### Dixon's Q test

Sort the data such that  

$$x_1 < x_2 < \ldots < x_n$$

The Q-value for $x_1$ is defined as  

$$Q = \frac{x_2 - x_1}{x_n - x_1}$$

The Q-value for $x_n$ is defined as  

$$Q = \frac{x_n - x_{n-1}}{x_n - x_1}$$

$x_1$ and $x_n$ could be labeled as potential outliers if the Q-value for them is greater than a pre-defined critical Q-value. Dixon provides an table of example critical Q-values for CL 90%, 95% and 99% with $N$ from $3$ to $10$.  

| $N$ | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
| :--: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: |
| $Q_{90\%}$ | 0.941 | 0.765 | 0.642 | 0.560 | 0.507 | 0.468 | 0.437 | 0.412 |
| $Q_{95\%}$ | 0.970 | 0.829 | 0.710 | 0.625 | 0.568 | 0.526 | 0.493 | 0.466 |
| $Q_{99\%}$ | 0.994 | 0.926 | 0.821 | 0.740 | 0.680 | 0.634 | 0.598 | 0.568 |

Dixon's Q test is recommended to never be used more than once on an observation.  

## Various sources

These sites helps Akatsumekusa tremendously when researching the topic.

* [NIST/SEMATECH Engineering Statistics Handbook](https://www.itl.nist.gov/div898/handbook/eda/section3/eda35h.htm)  
* [Wikipedia](https://en.wikipedia.org/wiki/Statistics)  
* [Educational Applets by Constantinos E. Efstathiou](http://195.134.76.37/applets/Applet_Index2.htm)  

Some contents in this note are directly copied from the following sites.  

* Z-score, Iglewicz and Hoaglin's modified Z-score: [NIST/SEMATECH Engineering Statistics Handbook](https://www.itl.nist.gov/div898/handbook/eda/section3/eda35h.htm) ([„Public information“](https://www.nist.gov/oism/copyrights))  
