---
layout: post
title: "Linear spatial filters (RESS) in analyzing steady-state evoked potentials: Study Notes"
date: 2019-11-07
excerpt: "RESS Case Study"
tags: [research project]
project: false
comments: false
---

A recent paper by Cohen and Gulbinaite introduced a procedure to analyze steady-state evoked potentials (SSEPs) which maximizes the
signal-to-noise ratio of the narrow-band steady-state response. The method, as they termed rhythmic entrainment source
separation (RESS), is conceptually similar to principal components analysis (PCA), which is covered in Week 12's class
and Cohen's book Chapter 23 as well. I find this new approach quite interesting and hope to write down something as to
make my understanding more organized.

## Principal Component Analysis
The general aim of the principal components analysis is focusing only on a fraction of variables to avoid overfitting
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
While PCA is a useful source separation technique, it is limited in a sense that all components are pairwise orthogonal.
Orthogonality is sometimes not helpful when we analyze brain activities, so a better way to do it is using generalized
eigendecomposition. 



Note that they used s

## Reference
Cohen, M. X., & Gulbinaite, R. (2017). Rhythmic entrainment source separation: Optimizing analyses of neural responses to rhythmic sensory stimulation. _Neuroimage_, 147, 43-56.
