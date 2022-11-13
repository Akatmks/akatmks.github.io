---
layout: post
title: Note on outliers in statistics [In Progress]
---

Knowledge on statistics is more important than any when it comes to the development of [Number Crunching Motion](https://github.com/Akatmks/Number-Crunching-Motion). This note lists all the possible ways Akatsumekusa found to remove outliers from a sample, and then explains Akatsumekusa's choice in the case of Number Crunching AAE Export.  

### Interquartile range

Outliers are defined as data that falls below $\mathit{Q1} - 1.5\mathit{IQR}$ or above $\mathit{Q3} + 1.5\mathit{IQR}$.  
with $\mathit{Q1}$ and $\mathit{Q3}$ denoting 25th and 75th percentile of the sample respectively, and $\mathit{IQR}$ represents the interquartile range and given by $\mathit{Q3} - \mathit{Q1}$.  

