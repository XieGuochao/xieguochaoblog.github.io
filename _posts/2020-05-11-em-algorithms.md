---
date: 2020-05-11 03:15:36
layout: post
title: "EM Algorithms"
subtitle:
description: Introduction to EM Algorithms, gaussian mixture model (GMM) and k-means algorithm. It includes some mathematical deductions.
image: /assets/img/background/our_planet_scenes__349_.jpg
optimized_image:
category: course
tags:
  - course
  - machine learning
  - CSC4020
author: Guochao Xie
paginate: false
math: true
---

(Note: The following materials are based on Prof. Zha, CSC4020, Spring 2020 and Kevin P. Murphy, Machine Learning: A Probabilistic Perspective.)

- [Mixture Models](#mixture-models)
  - [Gaussian Mixture Model](#gaussian-mixture-model)
  - [Using mixture models for clustering](#using-mixture-models-for-clustering)
    - [Soft clustering](#soft-clustering)
    - [Hard clustering](#hard-clustering)
  - [Log-Likelihood](#log-likelihood)
  - [Expectation Maximization (EM)](#expectation-maximization-em)
    - [Auxiliary function](#auxiliary-function)
    - [Algorithm](#algorithm)
    - [E Step](#e-step)
    - [M Step](#m-step)
  - [GMM EM Algorithm](#gmm-em-algorithm)
    - [E Step (GMM)](#e-step-gmm)
    - [M Step (GMM)](#m-step-gmm)
      - [MLE for MVN](#mle-for-mvn)
  - [K-means Algorithm](#k-means-algorithm)
    - [E step (k-means)](#e-step-k-means)
    - [M step (k-means)](#m-step-k-means)

---

# Mixture Models

Mixture modesl is a form of **latent variable model (LVM)**, representing a _discrete latent state_. We let $\pi_i$ be the "proportion" of the $i$th base distribution. Suppose we have $K$ such base distributions, then

$p(x_i\|\theta) = \sum_{k=1}^K \pi_k p_k (x_i \| \theta)$ where $\sum_{k=1}^K = 1$ and $0\le \pi_k \le 1$

For mixture models, we can have arbitary base distributions, for example, [**mixture of Gaussians (MOG)** or **Gaussian mixture model (GMM)**](#gaussian-mixture-model).

## Gaussian Mixture Model

For each base distribution, we have $p_k(x_i\|\theta) = N(x_i\|\mu_i, \Sigma_i)$. Then, we will have $p(x_i\|\theta) = \sum_{k=1}^K \pi_k N(x_i\|\mu_k, \Sigma_k)$. Given a sufficient _large number_ of $K$, GMM can be used to approximate _any density_ defined on $R^D$.

![Mixture Gaussian Demo](/assets/img/contents/mixture-gaussian-demo.jpg)

## Using mixture models for clustering

Since we have $K$ base distributions, it is nature to connect it with clustering by considering them as $K$ centroids.

### Soft clustering

For soft clustering, we define the **responsibility** of cluster $k$ for point $i$:

$$r_{ik} \triangleq p(z_i = k\| x_i, \theta) = \frac{p(z_i=k\|\theta) p(x_i\z_i=k, \theta)}{\sum_{k'=1}^K p(z_i=k'\|\theta) p(x_i\|z_i=k', \theta)}$$

An example for this is [GMM](#gaussian-mixture-model).

### Hard clustering

We use the **MAP** estimate for hard clustering, that is, instead of assigning weights with sum equals to 1, we assign a specific cluster for a data point and let others' weights to be 0:

$z_i^* = \arg \max_k \log p(x_i\|z_i=k, \theta) + \log p(z_i=k\|\theta)$

An example for this is [K-means](#k-means-algorithm).

## Log-Likelihood

For **MAP**, we are trying to maximize the log-likelihood of the dataset given $\theta$, that is

$\log p(D\|\theta)
= \log [p(D_1\|\theta) p(D_2\|\theta) ... p(D_N\|\theta)]$

$= \sum_i \log [p(D_i\|\theta)]$

$= \sum_i \log[\sum_{z_i} p(x_i, z_i\|\theta)]$

However, a challenge here is that we cannot push the log inside the sum. To solve it, with the assumption that $p(z_i, x_i \|\theta)$ is in the exponential family, including Dirichlet, multinomial, Gamma, Wishart, etc., we can use the **complete data log likelihood** as follows:

**Exponential family**: $p(x, z\|\theta) = \frac{1}{Z(\theta)} \exp [\theta^T \phi(x, z)]$. Here $Z(\theta)$ is the normalization term.

**Complete data log likelihood**: $l_c(\theta) = \sum_i \log p(x_i, z_i \| \theta) = \theta ^T(\sum_i \phi(x_i, z_i)) - NZ(\theta)$

With missing data, we have the **observed data log likelihood**: $l(\theta) = \sum_i \log \sum_{z_i} p(x_i, z_i\|\theta) = \sum_i \log [\sum_{z_i} e^{\theta ^T \phi(x_i, z_i}] - N\log Z(\theta)$

## Expectation Maximization (EM)

- $x_i$: the visible or observed variables in case $i$
- $z_i$: the hidden or missing variables
- Goal: maximizing the log likelihood of the observed data:

$\arg \max_\theta l(\theta) = \arg \max_\theta \sum_{i=1}^N \log p(x_i\|\theta) = \arg \max_\theta \sum_{i=1}^N \log[\sum_{z_i} p(x_i, z_i\|\theta)]$

We use the **complete data log likelihood** instead:

$l_c(\theta) = \sum_{i=1}^N \log p(x_i, z_i\|\theta)$

However, this cannot be computed since $z_i$ is unknown. We define **expected complete data log likelihood** as follows:

$Q(\theta, \theta^{t-1}) = E[l_c(\theta) \| D, \theta^{t-1}]$

- $t$: the current iteration number.
- $Q$: **auxiliary function**.

Here, we use the expectation so that we can turn $I(z_i = k)$ into $E[I(z_i = k)] = p(z_i=k\| x_i, \theta^{t-1}) = r_{ik})$.

### Auxiliary function

$Q(\theta, \theta^{t-1})$

$\triangleq E[\sum_i \log p(x_i, z_i\|\theta)]$

$= \sum_i E[\ \log [\ \Pi_{k=1}^K (\pi_k p(x_i \| \theta_k))^{I(z_i = k)}\ ] \ ]$

$= \sum_i \sum_k E[\ I(z_i = k)\ ]\ \log[\ \pi_k p(x_i\|\theta_k)\ ]$

$= \sum_i \sum_k p(z_i=k\|x_i, \theta^{(t-1)})\ \log[\ \pi_k p(x_i\|\theta_k)\ ]$

$= \sum_i \sum_k r_{ik} \log \pi_k + \sum_i \sum_k r_{ik} \log p(x_i\|\theta_k)$

- $r_{ik} \triangleq p(z_i=k \| x_i, \theta^{(t-1)})$

### Algorithm

The **EM algorithm** is as follows:

1. Initialize $\theta^0$.
2. **E Step**: Calculate $Q(\theta, \theta^{t-1})$ and $r_{ik}$.
3. **M Step**:
   1. $\theta^t = \arg \max_\theta Q(\theta, \theta^{t-1})$ (**MLE**)
   2. or $\theta^t = \arg \max_\theta Q(\theta, \theta^{t-1}) + \log p(\theta)$ (**MAP**)
4. Do step 2 and 3 until convergence.

### E Step

$r_{ik} = \frac{\pi_k p(x_i\|\theta_k^{(t-1)} )}{\sum_{k'} \pi_{k'} p(x_i \| \theta_{k'}^{(t-1)})}$

### M Step

$\pi_k = \frac{1}{N} \sum_i r_{ik} = \frac{r_k}{N}$

- $r_k \triangleq \sum_i r_{ik}$: the _weighted_ _number of points_ assigned to cluster $k$.

## GMM EM Algorithm

### E Step (GMM)

The same with the general case:

$r_{ik} = \frac{\pi_k p(x_i\|\theta_k^{(t-1)} )}{\sum_{k'} \pi_{k'} p(x_i \| \theta_{k'}^{(t-1)})}$

Here we have the gaussian mixture model, so

$p(x_i\|\theta) = \sum_{k=1}^K \pi_k p_k (x_i \| \theta)$

$= \sum_{k=1}^K \pi_k [\ (2\pi)^{-\frac{D}{2}} \|\Sigma_k\|^{-\frac{1}{2}} \exp (-\frac{1}{2} (x_i-\mu_k)^T\Sigma_k^{-1}(x_i-\mu_k))\ ]$

is about $\mu_k$ and $\Sigma_k\ \forall k$.

### M Step (GMM)

$\pi_k = \frac{1}{N}\sum_i r_{ik} = \frac{r_k}{N}$

- $r_k \triangleq \sum_i r_{ik}$: the _weighted_ _number of points_ assigned to cluster $k$.

For part of $Q$:

$l(\mu_k, \Sigma_k)$

$= \sum_k \sum_i r_{ik} \log p(x_i\|\theta_k)$

$= -\frac{1}{2} \sum_i r_{ik} [\ \log \|\Sigma_k\| + (x_i - \mu_k)^T \Sigma_k^{-1}(x_i - \mu_k)\ ]$

The **MLE** results:

$\mu_k = \frac{\sum_i r_{ik}x_i}{r_k}$

$\Sigma_k= \frac{\sum_i r_{ik}(x_i - \mu_k) (x_i - \mu_k)^T}{r_k}$

$= \frac{\sum_i r_{ik}(x_ix_i^T - x_i\mu_k^T - \mu_k x_i^T + \mu_k \mu_k^T)}{r_k}$

$= \frac{\sum_i r_{ik}(x_ix_i^T - 2\mu_k\mu_k^T + \mu_k \mu_k^T)}{r_k}$

$= \frac{\sum_i r_{ik} x_ix_i^T}{r_k} - \mu_k \mu_k^T$

---

#### MLE for MVN

(Reference: [https://www.cs.princeton.edu/~bee/courses/scribe/lec_09_09_2013.pdf](https://www.cs.princeton.edu/~bee/courses/scribe/lec_09_09_2013.pdf))

A **multivariate normal distribution (MVN)** follows the pdf:

$N(x\| \mu, \Sigma) \triangleq \frac{1}{(2\pi)^{d/2} \|\Sigma\|^{1/2}} \exp[-\frac{1}{2}(x-\mu)^T \Sigma^{-1}(x-\mu)]$

- $\Sigma$ is a _symmetric_ _positive definite_ matrix.
- Trace of $A$: $tr(A) = \sum_i^d A_{ii}$.
- $\|A\| \in R^{d\times d} = \Pi v_i$ where $v_i$ are the eigenvectors. A positive definite matrix has *positive eigenvalues* so the determinant will always be *positive*.
- Symmetric positive definite $\implies$ eigenvalues strictly positive.
- $tr(ABC) = tr(CAB) = tr(BCA)$.

Facts for matrix derivatives:

1. $\frac{\partial b^Ta}{\partial a} = b$
2. $\frac{\partial(a^TAa)}{\partial a} = (A + A^T)a$. If $A$ is symmetric, $=2Aa$.
3. $\frac{\partial tr(BA)}{\partial A} = B^T$
4. $\frac{\partial \log \|A\|}{\partial A} = A^{-T}$
5. Trace: $a^TAa = tr(a^TAa) = tr(aa^TA) = tr(Aaa^T)$

The MLE for a data set $D$ given $\mu, \Sigma$: 

$L(\mu, \Sigma; D) = \log \Pi_{i=1}^n p(x_i\|\mu, \Sigma)$

$= \frac{n}{2} \log \|\Sigma^{-1}\| - \frac{1}{2}\sum_{i=1}^n (x_i - \mu)^T\Sigma^{-1} (x_i - \mu)$

$= \frac{n}{2} \log \|\Sigma^{-1}\| - \frac{1}{2}\sum_{i=1}^n tr[(x_i - \mu)(x_i-\mu)^T \Sigma^{-1}]$

Set the partial derivative w.r.t. $\mu$ to 0:

$\frac{\partial L(\mu, \Sigma; D)}{\partial \mu} = -\frac{1}{2}\sum_{i=1}^n -2 \Sigma^{-1}(x_i-\mu) = 0$ (use rule 2).

Then, we get 

$$\sum_{i=1}^n(x_i-\mu) = 0\\
\implies \sum_{i=1}^n x_i - \sum_{i=1}^n \mu = 0\\
\implies \mu_{MLE} = \frac{1}{n} \sum_{i=1}^n x_i$$

If we have weights for $x_i$, we only need to add weights to $x_i$ to get the new $\mu_{MLE}$.

Similarly, we set the partial derivative w.r.t. $\Sigma^{-1}$ to 0.

$\frac{\partial L(\mu; \Sigma; D)}{\partial \Sigma^{-1}} = \frac{n}{2}\Sigma - \frac{1}{2}(\sum_{i=1}^n (x_i - \mu)(x_i-\mu)^T) = 0$ (use rule 3 and 5)

$\Sigma_{MLE} = \frac{1}{n} \sum_{i=1}^n \sum_{i=1}^n (x_i-\mu)(x_i-\mu)^T$

---

## K-means Algorithm

Assume $\Sigma_k = \sigma^2 I_D$ is fixed and $\pi_k = 1 / K$ is fixed. Then, we only have to estimate the cluster centers $\mu_k \in R^D$.

### E step (k-means)

$p(z_i = k \| x_i, \theta) \approx I (k=z_i^*)$ 

where $z_i^* = \arg \max_k p(z_i = k \| x_i, \theta) = \arg \max_k \|\|x_i - \mu_k \|\|_2^2$. ([**hard EM**](#hard-clustering))

### M step (k-means)

$\mu_k = \frac{1}{N_k} \sum_{i:z_i = k} x_i$ to calculate the centroids.

---