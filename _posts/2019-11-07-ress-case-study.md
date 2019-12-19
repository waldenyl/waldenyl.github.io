---
layout: post
title: "Linear spatial filters (RESS) in analyzing steady-state evoked potentials: Study Notes"
date: 2019-11-07
excerpt: "RESS Case Study"
tags: [research project]
comments: false
---

<p align=center> Last Update: 12/19/2019 </p>

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

## Generalized Eigendecomposition
In a recent study on steady-state evoked potentials (SSEPs), Cohen and Gulbinaite (2017) used generalized eigendecomposition to facilitate the time-frequency analysis on neural responses to rhythmic sensory stimulation. Eigendecomposition is the main idea underlying principal component analysis (PCA), where the eigenvector pointing to the direction of the maximal variance of the data is found by examining the covariance matrix. Previous studies have used different source separation techniques, including PCA (Bernat et al., 2007; Bernat, Williams, and Gehring, 2005). However, PCA has its bad performance in source separation in neural signals. While most brain dynamics are not in an orthogonal pattern, PCA is a blind source separation technique that produces pairwise orthogonal eigenvectors. Studies have shown that principal component analysis gives poor results compared to other procedures that do not require orthogonality (e.g., Delorme et al., 2012; Dien et al., 2007).

One way to overcome this constraint is to use generalized eigendecomposition (GED). GED first creates two covariance matrices for two features, one serving as the signal and one as the reference, and uses eigendecomposition to find out a set of eigenvectors that maximally differentiate activity between these two different features, which can be used as spatial filters to increase the signal-to-noise ratio. Therefore, for our final analysis, we attempted to use GED to isolate alpha-band activity and compare between different experimental conditions (i.e. oddball vs common).

## Applying Generalized Eigendecomposition to Analyzing EEG Time-Frequency Signals
Here I introduce what we did in analyzing the data from the oddball task. The procedure of using generalized eigendecomposition to derive a spatial filter and applying it to the raw data. First, we applied a narrow band-pass filter centered at 7 Hz with a full width at half maximum (FWHM) of 5 Hz. Noticeably, Cohen & Gulbinaite’s (2017) preferred this Gaussian-shaped filter and time-domain covariance because in their study, they used stimuli flickering at different frequencies that will elicit rhythmic brain responses (steady-state evoked potentials), which are not stationary nor sinusoidal. Here we also used a Gaussian filter to account for non-stationarity.

From the filtered data, we constructed two covariance matrices, a pre-stimulus one with a time window from -500 ms to 0 ms and a post-stimulus one with a time window from 0 ms to 500 ms. Then, we performed a generalized eigendecomposition to extract the largest eigenvectors that differentiate pre-stimulus and post-stimulus response. We then multiplied the component weights with the raw data and showed the topographical characteristics of the components. Finally, we applied a Hilbert transformation analysis to the EEG data filtered by the component weights. For the power correction of the result, we used -300 ms to 0 ms as the baseline.


## Reference
Cohen, M. X., & Gulbinaite, R. (2017). Rhythmic entrainment source separation: Optimizing analyses of neural responses to rhythmic sensory stimulation. _Neuroimage_, 147, 43-56.
