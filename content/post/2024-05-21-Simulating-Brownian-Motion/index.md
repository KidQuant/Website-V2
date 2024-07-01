---
title: "Simulating Geometric Brownian Motion"
date: "2024-05-21T00:00:00Z"
authors:
  - admin

categories:
  - Finance
  - Programming

tags:
  - Quant Finance

summary:  

featured: true
reading_time: true
draft: false

markup: mmark

image:
  placement: 2
  preview_only: false

math: true
---

### Confidence intervals

So far, the stuff we've seen is pretty easy. However, we're trying to be precise. We want to associate a confidence interval to each estimated parameter. In other words, we want to indicate how close the sample statistic (the estimate parameter) is to the population parameter (the real value of the parameter).

We can find an interval of values that contains the real parameter, with a confidence level of $(1-\alpha)100\%$, for $0<\alpha<1$. This is called the *confidence interval*.

Let us introduce some absic concepts:

Let $X_i$ be independent and identically distributed (i.i.d), for $1\leq i\leq n$, with $\mathbb{E}[X_i]=\mu$ and $\textit{Var}[X_i]=\sigma^2$. We call $\bar{X}=\frac{1}{n}\sum_{i=1}^nX_i$.