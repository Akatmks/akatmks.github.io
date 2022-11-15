---
layout: post
title: Note on outliers in statistics [In writing]
---

Knowledge on statistics is more important than any when it comes to [Number Crunching Motion](https://github.com/Akatmks/Number-Crunching-Motion). This note summarises Akatsumekusa's finding when looking for methods to remove outliers from a sample, and explains Akatsumekusa's choice in the case of Number Crunching AAE Export (NC AAE Export).  

Note that the way Akatsumekusa and NC AAE Export views and treats outliers is different than that of a scientific research. In a scientific research, the researchers could draw two graphs next to each other, one without the outliers and one with. However, NC AAE Export can return one and only one point estimate.  

## Detection of outliers

There are three basic ideas of outliers detection, deviation-based detection, interquartile-range-based detection, excess-based detection.  

### Deviation-based detection

In deviation-based detection, a data could be labeled as potential outlier if it deviates too far away from the observation average. There are two major variables when it comes to deviation-based detections, the average and the critical threshold.  

Z-score as well as a couple of commonly used formal methods like Grubbs' test, Tietjen-Moore test and Rosner's test are based on mean and standard deviation, while Iglewicz and Hoaglin's modified Z-score is based on median and median absolute deviation. There are few methods based on mode.  

#### Z-score

Z-score of an observation is defined as  

$$Z_i = \frac{x_i - \bar{x}}{s}$$

with $\bar{x}$ and $s$ denoting the sample mean and sample standard deviation respectively. Data with a Z-score with an absolute value of greater than $3$ could be labelled as potential outliers.  
Z-score could be misleading as the maximum Z-score for an observation is at most $(N - 1) / \sqrt{N}$.  

#### Iglewicz and Hoaglin's modified Z-score

The Z-score of an observation is defined as  

$$M_i = \frac{0.6745(x_i - \tilde{x})}{\mathit{MAD}}$$

with $\tilde{x}$ denoting the sample median and $\mathit{MAD}$ representing the median absolute deviation. Iglewicz and Hoaglin recommends that modified Z-scores with an absolute value of greater than $3.5$ be labelled as potential outliers.  

The same exact method is also seen in Hampel filter and Huber's method, albeit with different critical thresholds.  

#### Grubbs' test

Grubbs' test statistic is defined as  

$$G = \frac{\max\lvert x_i - \bar{x} \rvert}{s}$$

with $\bar{x}$ and $s$ denoting the sample mean and sample standard deviation respectively. Such $x_i$ with $\max\lvert x_i - \bar{x} \rvert$ could be labelled as potential outliers if  

$$G > \frac{N - 1}{\sqrt{N}} \sqrt{\frac{(t_{\alpha/(2N), N-2})^2}{N-2+(t_{\alpha/(2N),N-2})^2}}$$

with $t_{\alpha/(2N),N-2}$ denoting the critical value of the t distribution with $(N-2)$ degrees of freedom and a significance level of $\alpha/(2N)$.  
Grubbs' test could be iterated repeatly until no outliers are detected. Simulation studies suggests Grubbs's test not to be used in observations where $N \leq 6$.  

#### Tietjen-Moore test

Compute the absolute residuals

$$r_i = \lvert x_i - \bar{x} \rvert$$

with $\bar{x}$ denoting the sample mean. Let $z_i$ denote the $x_i$ values sorted by their absolute residuals in ascending order, and the test statistics for $k$ points with the largest absolute residuals is

$$E_k = \frac{\sum_{i=1}^{n-k} (z_i-\bar{z}_k)^2}{\sum_{i=1}^n (z_i-\bar{z})^2}$$

with $\bar{z}$ denoting the sample mean for the full dataset and $\bar{z}_k$ denoting the sample mean with the $k$ points with the largest absolute residuals removed. Such $k$ points could be labelled as potential outliers if $E_k$ is close to $1$.  

#### Rosner's generalized extreme studentized deviate (ESD) test

Compute the absolute residuals

$$r_i = \lvert x_i - \bar{x} \rvert$$

with $\bar{x}$ denoting the sample mean. Let $z_i$ denote the $x_i$ values sorted by their absolute residuals in decending order. Compute

$$R_i = \frac{\lvert z_i - \bar{z} \rvert}{s}$$

$$\lambda _i = \frac{(N - i) t_{1-\alpha/[2(N-i+1)],N-i-1}}{\sqrt{(N-i-1+(t_{1-\alpha/[2(N-i+1)],N-i-1})^2)(N-i+1)}}$$

with $\bar{z}$ and $s$ denoting the sample mean and sample standard deviation respectively, and $t_{1-\alpha/[2(n-i+1)],N-i-1}$ denoting the critical value of the t distribution with $(N-i-1)$ degrees of freedom and a significance level of $1-\frac\alpha{2(N-i+1)}$. The first $k$ points could be labeled as potential outliers if  

$$R_i > \lambda_i \quad \mathrm{for}\; i=1,2, \ldots ,k$$

Simulation studies suggests the critical value approximation in Rosner's test to be very accurate for $N \geq 25$ and critical value approximation for $N \geq 15$.  

### Interquartile-range-based detection

In interquartile-range-based detection, a data could be labeled as potential outlier if it deviates too far away from the lower or upper quartile.  
A commonly used critical threshold for interquartile-range-based detection is $1.5\mathit{IQR}$. Data that falls below $Q_1 - 1.5\mathit{IQR}$ or above $Q_3 + 1.5\mathit{IQR}$ could be labelled as potential outliers, with $Q_1$ and $Q_3$ denoting the lower and upper quartile respectively, and $\mathit{IQR}$ representing the interquartile range.  

### Excess-based detection
In excess-based detection, a data could be labeled as potential outlier if the distance between it and the data before or next to it is larger than a critical threshold.  

#### Dixon's Q test

Sort the data such that  

$$x_1 < x_2 < \ldots < x_n$$

The Q-value for $x_1$ is defined as  

$$Q = \frac{x_2 - x_1}{x_n - x_1}$$

The Q-value for $x_n$ is defined as  

$$Q = \frac{x_n - x_{n-1}}{x_n - x_1}$$

$x_1$ and $x_n$ could be labelled as potential outliers if the Q-value for them is greater than a pre-defined critical Q-value. Dixon provides an table of example critical Q-values for 90%, 95% and 99% confidence level with $N$ ranging from $3$ to $10$.  

| $N$ | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
| :--: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: |
| $Q_{90\%}$ | 0.941 | 0.765 | 0.642 | 0.560 | 0.507 | 0.468 | 0.437 | 0.412 |
| $Q_{95\%}$ | 0.970 | 0.829 | 0.710 | 0.625 | 0.568 | 0.526 | 0.493 | 0.466 |
| $Q_{99\%}$ | 0.994 | 0.926 | 0.821 | 0.740 | 0.680 | 0.634 | 0.598 | 0.568 |

Dixon's Q test is recommended to never be used more than once on a dataset.  

## The case of Number Crunching AAE Exprot

*As the development of Number Crunching AAE Export is still ongoing at the moment, there may be more contents to be added to this note.*  

There are currently ~~3~~ cases in the Number Crunching AAE Export that requires outliers to be removed.  

### ~~Step 18~~

~~The first case is at step 18 when it needs to check through all the frames and decide if each frame should be sent to functions that deals with pure x/y movements or functions that deals with scale x/y movements. This is the very first real calculation to do in the whole process of NC AAE Export (although not the first potentially slow step; step 04 which reads the tracking data from Blender into numpy array is extremely slow due to the use of Python for). This step doesn't need to be elegant but it needs to be finished as fast as possible without missing any scale frames.~~  

~~It works as such: First, it generates a number of random pairs of tracks for the frame from the position/movement array, and then it calculates the intersection for each pairs of position/movement. If all of the intersections are centred around a single point in the 2D space, it will be considered a frame with scaling and should be sent to the scale x/y functions. If otherwise the intersections are scattered around the space and especially if there are a lot of parallel pairs of movements, it should be considered a frame without scaling and be sent to the pure x/y functions.~~  

~~The problem with this step is that the original tracking data likely contains a large number of tracking errors, which will very much mess up the intersection calculation. Instead of trying to remove outliers and calculate average deviation, it would be better to~~ 

Akatsumekusa just realised that there are machine learning models for clustering and there is a good library called scikit-learn.  

## Various sources

These sites and books help Akatsumekusa tremendously when researching the topic.

* [Outliers in statistical data (1984) by Vic Barnett](https://archive.org/details/outliersinstatis0000barn_k7p8/page/n7/mode/2up)
* [NIST/SEMATECH Engineering Statistics Handbook](https://www.itl.nist.gov/div898/handbook/eda/section3/eda35h.htm)  
* [Wikipedia](https://en.wikipedia.org/wiki/Statistics)  
* [Educational Applets by Constantinos E. Efstathiou](http://195.134.76.37/applets/Applet_Index2.htm)  

Some contents in this note are directly copied from the following sites.  

* Z-score, Iglewicz and Hoaglin's modified Z-score, Grubbs' test, Tietjen-Moore test, Rosner's generalized ESD test: [NIST/SEMATECH Engineering Statistics Handbook](https://www.itl.nist.gov/div898/handbook/eda/section3/eda35h.htm) ([„Public information“](https://www.nist.gov/oism/copyrights))  
