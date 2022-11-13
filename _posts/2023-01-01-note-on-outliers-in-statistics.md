---
layout: post
title: Note on outliers in statistics
---

When developing [Number Crunching Motion](https://github.com/Akatmks/Number-Crunching-Motion), Akatsumekusa realised that knowledge on statistics is more important than any. This note lists all the possible ways Akatsumekusa found to remove outliers from a sample, and then explains Akatsumekusa's choice in the case of Number Crunching AAE Export.  

### Interquartile range

Outliers are defined as data that falls below ${Q1} - 1.5{IQR}$ or above ${Q3} + 1.5{IQR}$.  
with ${Q1}$ and ${Q3}$ denoting 25th and 75th percentile of the sample, respectively, and ${IQR}$ represents the interquartile range and given by ${Q3} - {Q1}$.  

