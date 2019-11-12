---
layout: page
title: "Linear spatial filters (RESS) in analyzing steady-state evoked potentials: Notes and comments on Cohen's procedure"
date: 2019-11-07
excerpt: "RESS Case Study"
tags: [research project]
project: true
comments: false
---

We just discussed principal component analysis (PCA) in class this afternoon and I didn't quite follow. But I nevertheless feel the whole
idea interesting, so maybe I will start a new thread to write down some notes.

## Principal Component Analysis
The main idea of the principal components analysis is focusing only on a fraction of variables to avoid overfitting
by reducing the dimension of the input space. If one variable accounts for a major proportion of the variance, it must
be more important than another variable that only accounts for a small proportion of the variance. For example, in a
visual search task, a lot of factors may influence a participant's mean reaction time in finding a target. If we could
find some factors that correlate to a wide range of variability in reaction times, these are factors more critical to
to task.

```
erp = squeeze(mean(EEG.data,3));
erp = bsxfun(@minus,erp,mean(erp,2));
```

Compute the covariance of ERP. Since we are interested in channel covariance, we calculate it using A*A'. This should
give us a 32*32 covariance matrix, which is normalized by N - 1 = 31.

```
covar = (erp*erp')./(EEG.pnts-1);
```

## PCA as Spatial Filters in EEG Data
If we have 32 electrodes in 

## Generalized Eigendecomposition
While PCA is a useful source separation technique, it is 
