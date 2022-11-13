---
layout: post
title: Note on outliers in statistics [In Progress]
---

Knowledge on statistics is more important than any when it comes to the development of [Number Crunching Motion](https://github.com/Akatmks/Number-Crunching-Motion). This note lists all the possible ways Akatsumekusa found to remove outliers from a sample, and then explains Akatsumekusa's choice in the case of Number Crunching AAE Export.  

### Interquartile range

Outliers are defined as data that falls below $\mathit{Q1} - 1.5\mathit{IQR}$ or above $\textit{Q3} + 1.5\textit{IQR}$.  
with ${Q1}$ and ${Q3}$ denoting 25th and 75th percentile of the sample, respectively, and ${IQR}$ represents the interquartile range and given by ${Q3} - {Q1}$.  

