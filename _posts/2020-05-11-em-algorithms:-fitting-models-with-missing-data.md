---
date: 2020-05-11 15:50:45
layout: post
title: "EM Algorithms: Fitting Models with Missing Data"
subtitle:
description: EM algorithm's application in fitting models with missing data.
image: /assets/img/background/Hyades_Mtanous_1998.jpg
optimized_image:
category: course
tags:
  - course
  - CSC4020
  - machine learning
author: Guochao Xie
paginate: false
math: true
---

# Problem of fitting models with missing data

Let $O_{ij} = 1$ if feature $j$ of data sample $i$ is observed, $O_{ij} = 0$ otherwise. $X_v = \{x_{ij}: O_{ij} = 1\}$ and $X_h = \{x_{ij}: O_{ij} = 0\}$.

- Goal: $\hat{\theta} = \arg \max_\theta p(X_v \| \theta, O)$.

- Assumption: $p(X_v\|\theta, O) = \Pi_{i=1}^N p(x_i^v \| \theta)$

  - $x_i^v$ is the vector $x_i$ with missing data deleted.

## Log-likelihood

$\log p(X_v \| \theta) = \sum_i \log p(x_i^v\|\theta) = \sum_i \log[\sum_{x_i^h} p(x_i^v, x_i^h\|\theta)]$

---

# MVN Example

We will illustrate the **MVN** example because it is in exponential family and thus we can take the $\log$ out.

---

## E step (MVN)

The expected complete data log likelihood at iteration $t$:

$Q(\theta, \theta^{t-1}) = E[\sum_{i=1}^N \log N(x_i \| \mu, \Sigma) \| D, \theta^{t-1}]$

$= -\frac{N}{2}\log \| 2\pi \Sigma\| - \frac{1}{2} \sum_{i=1}^N E[(x_i - \mu)^T \Sigma^{-1}(x_i-\mu)]$

$= -\frac{N}{2}\log \|\Sigma\| - \frac{ND}{2} \log 2\pi - \frac{1}{2} tr(\Sigma^{-1} E[S(\mu)])$

- $E[S(\mu)] = \sum_{i=1}^N (E[x_i x_i^T] + \mu \mu^T - 2\mu E[x_i]^T])$

Then, we need to compute $E[x_i x_i^T]$ and $E[x_i]$. Consider MVG:

$x_i^h \| x_i^v, \theta \sim N(m_i, V_i)$

where 

- $m_i = \mu_h + \Sigma_{hv} \Sigma_{vv}^{-1}(x_i^v - \mu_v)$
- $V_i = \Sigma_{hh} - \Sigma_{hv} \Sigma_{vv}^{-1}\Sigma_{vh}$
- Here, $\Sigma_{hh}, \Sigma_{hv}, \Sigma_{vv}, \Sigma_{vh}$ are 4 components of the $\Sigma$. 

Hence, $E[x_i] = (E[x_{ih}; x_{iv}]) = (m_i; x_{iv})$, that is, for the missing data we use $m_i$ to represent; for the visible data, we directly use $x_{iv}$.

To compute $E[x_ix_i^T]$ which is a bit more complicated, we use the result that $cov[x] = E[xx^T] - E[x]E[x^T]$. Hence,

$E[x_i x_i^T] = E[\begin{pmatrix}x_{ih}\\\\ x_{iv}  \end{pmatrix} (x_{ih}^T\ x_{iv}^T)] = \begin{pmatrix} E[x_{ih}x_{ih}^T]\quad E[x_{ih}x_{iv}^T]x_{iv}^T \\\\ x_{iv} E[x_{ih}]^T\quad x_{iv}x_{iv}^T  \end{pmatrix}$

and $E[x_{ih}x_{ih}^T] = E[x_{ih}]E[x_{ih}]^T + V_i$.

---

## M step (MVN)

Plug the ESS into the usual MLE equations:

- $\mu^t = \frac{1}{N} \sum_i E[x_i]$
- $\Sigma^t = \frac{1}{N} \sum_i E[x_i x_i^T] - \mu^t (\mu^t)^T$

Here, $E[x_i] = (E[x_{i}^h]\quad x_i^v)$ and $E[x_i x_i^T]$ are calculated in [E step](#e-step-mvn).

---

## Python Codes

This will be released soon.