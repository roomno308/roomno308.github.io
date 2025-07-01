---
layout: post
title: Chapter 6 - Stochastic Approximation
author: Chaitanya
---

## Motivating Example: Mean Estimation

SUppose we have a random variable \\(X\\) which takes values from a finite set \\( \mathcal{X} \\). We want to estimate the mean \\( \mathbb{E}[X] \\) of this random variable. Suppose we also get i.i.d samples \\( \{x_i\}_{i=1}^n \\). In the [last chapter](ch5.html), we saw that we can estimate the mean using monte carlo estimation as follows:

$$
\begin{equation*}
\mathbb{E}[X] \approx \frac{1}{n} \sum_{i=1}^n x_i
\end{equation*}
$$

Even thouth this estimation becomes more accurate as \\(n \to \infty \\), it is not practical to wait for infinite samples. To overcome this, we can use an incremental update mechansism to update the estimate $w_k$ of the mean after every sample. We know that:

$$
\begin{equation*}
\begin{split}
    w_{k} &= \frac{1}{k-1} \sum_{i=1}^{k-1} x_i \hspace{1em} k = 2,3,\ldots \\
    w_{k+1} &= \frac{1}{k} \sum_{i=1}^k x_i, \hspace{1em} k = 1,2,\ldots
\end{split}
\end{equation*}
$$

Then, \\(w_{k+1} \\) can be written as:

$$
\begin{equation*}
\begin{split}
    w_{k+1} &= \frac{1}{k} \left( \sum_{i=1}^{k-1} x_i + x_k \right) \\
    &= \frac{1}{k} \left( (k-1) w_k + x_k \right) \\
    &= w_k - \frac{1}{k} (w_k - x_k)
\end{split}
\end{equation*}
$$

Therefore, we get our incremental algorithm for estimating the mean:

$$
\begin{equation*}
w_{k+1} = w_k + \frac{1}{k} (x_k - w_k)
\end{equation*}
$$

Even though, it is obvious that the initial estimates of the mean will be inaccurate, this algorithm will still converge to the true mean as \\(k \to \infty\\). Furthermore, we won't have to store all the samples, and wait for all the samples to arrive before we can update our estimate.

A more general form of this algorithm is:

$$
\begin{equation*}
w_{k+1} = w_k + \alpha_k (x_k - w_k)
\end{equation*}
$$

Here, the term \\( \frac{1}{k} \\) is replaced by a step size \\( \alpha_k > 0 \\). We will learn in this chapter that if \\( \alpha_k \\) satisfies some (very mild) conditions, then the algorithm will still converge to the true mean.