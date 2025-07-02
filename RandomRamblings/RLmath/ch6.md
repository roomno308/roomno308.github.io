---
layout: post
title: Chapter 6 - Stochastic Approximation
author: Chaitanya
---

<div style="display: flex; justify-content: space-between;">
  <a href="ch5.html">&larr; Previous Chapter</a>
  <a href="ch7.html">Next Chapter &rarr;</a>
</div>


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
\begin{equation}
w_{k+1} = w_k + \alpha_k (x_k - w_k)
\label{Incremental Mean Estimation}
\end{equation}
$$

Here, the term \\( \frac{1}{k} \\) is replaced by a step size \\( \alpha_k > 0 \\). We will learn in this chapter that if \\( \alpha_k \\) satisfies some (very mild) conditions, then the algorithm will still converge to the true mean.

## Robbins-Monro Algorithm

Robbins-Monro (RM) algorithm is a stochastic roots finding algorithm and will later see that famous algorithms like Stochastic Gradient Descent (SGD) is a special case of this algorithm. Suppose that we need to find the root of the equation \\( g(w) = 0 \\), where \\( w \in \mathbb{R} \\) and \\( g: \mathbb{R} \to \mathbb{R} \\) is an unknown function. This means that we can't find a closed form solution for \\( w \\), neither can we calculate the derivative of \\( g(w) \\) to use other iterative methods. Furthermore, we can only obtain a noisy sample (with an observation error $\eta_k$) of the function value at a point \\( w \\) as follows:

$$
\begin{equation*}
\tilde{g}(w,\eta_k) = g(w_k) +  \eta_k
\end{equation*}
$$

![alt text](../graphics/RLmath/ch6RM1.png)

The RM algorithm that solves this problem is:

$$
\begin{equation}
w_{k+1} = w_k - \alpha_k \tilde{g}(w_k, \eta_k) \hspace{1em} k = 1,2,\ldots
\label{RMalgorithm}
\end{equation}
$$

Here, \\(w_k\\) is the estimate of the root at iteration \\(k\\) and \\(\alpha_k > 0\\) is a positive coefficient. It is obvious that this alorithm converges only when certain conditions are satisfied (one obvious condition is that \\(g(w)\\) is monotonically increasing). But, before we discuss the conditions, let us first understand the intuition behind this algorithm by the following examples:

Suppose \\(w^\*\\) is the root of the equation \\(g(w) = 0\\):

1. When \\(w_k > w^\*\\), we have \\(g(w_k) > 0\\). Then, \\( w_{k+1} = w_k - \alpha_k g(w_k) < w_k\\). This means that we are moving towards the root if \\( \| \alpha_k g(w_k) \| \\) is sufficiently small.
2. When \\(w_k < w^\*\\), we have \\(g(w_k) < 0\\). Then, \\( w_{k+1} = w_k - \alpha_k g(w_k) > w_k\\). This means that we are moving towards the root if \\( \| \alpha_k g(w_k) \| \\) is sufficiently small.

In either case, we are moving towards the root.

Now, we can formally discuss the conditions under which the RM algorithm converges:

> **Theorem (Robbins-Monro Theorem)**: The Robbins-Monro algorithm stated in Equation $\eqref{RMalgorithm}$ converges almost surely to the root \\(w^\*\\) of the equation \\(g(w) = 0\\) if:
> 1. \\( 0< c_1 \leq \nabla_w g(w) \leq c_2 < \infty \\) for all \\(w\\).
> 2. \\( \sum_{k=1}^{\infty} \alpha_k = \infty \\) and \\( \sum_{k=1}^{\infty} \alpha_k^2 < \infty \\).
> 3. \\( \mathbb{E}[\eta_k | \mathcal{H}_k] = 0 \\) and \\( \mathbb{E}[\eta_k^2 | \mathcal{H}_k] < \infty \\) 
> where \\(\mathcal{H}_k = \{w_1, \ldots, w_k\}\\) is the history of the algorithm up to iteration \\(k\\). 

> **Proof**: Refer to the book chapter for the proof.

The conditions are explained as follows:

1. \\( 0< c_1 \leq \nabla_w g(w) \\) indicates that the function \\(g(w)\\) is monotonically increasing. This ensures that the root exists and is unique. \\
Furthermore, as an application where \\( g(w) = \nabla_w J(w) \\) for some cost function \\(J(w)\\), this condition ensures that the cost function is convex, which is a commonly adopted assumption in optimization problems. \\
The condition \\( \nabla_w g(w) \leq c_2 < \infty \\) ensures that the function is not too steep, or the gradient is bounded from above.

2. The second condition \\( \sum_{k=1}^{\infty} \alpha_k = \infty \\) and \\( \sum_{k=1}^{\infty} \alpha_k^2 < \infty \\) ensures that the series converges, but not too fast.\\
First, \\( \sum_{k=1}^{\infty} \alpha_k^2 < \infty \\) suggests that \\(\alpha_k \to 0\\) as \\(k \to \infty\\). This is necessary because \\(w_{k+1} - w_k = -\alpha_k g(w_k)\\) should go to zero as \\(k \to \infty\\) for the algorithm to converge.\\
Second, \\( \sum_{k=1}^{\infty} \alpha_k = \infty \\) ensures that the steps don't converge too fast and we reach the root from arbitrarily far away.

3. The third condition is a mild technical condition: \\( \mathbb{E}[\eta_k \| \mathcal{H}_k] = 0 \\) ensures that the noise is unbiased, i.e., the expected value of the noise is zero. This is necessary for the algorithm to converge to the true root. The low variance condition \\( \mathbb{E}[\eta_k^2 \| \mathcal{H}_k] < \infty \\) ensures that the noise is not too large, which could otherwise prevent the algorithm from converging.

Now it can be clearly seen that our $\eqref{Incremental Mean Estimation}$ algorithm is a special case of the RM algorithm where \\(g(w) = w - \mathbb{E}[X]\\) and therefore it converges if the conditions of the RM theorem are satisfied.


## Dvoretzky's convergence theorem

This section is a detour from the main topic, but this, along with some other results, is important for understanding the convergence of RL algorithms like Q-learning. Dvoretzky's convergence theorem can also be used to prove the convergence of the Robbins-Monro algorithm.

> **Theorem (Dvoretzky's Convergence Theorem)**: consider a stochastic process
>
>$$
\begin{equation*}
    \delta_{k+1} = (1-\alpha_k) \delta_k + \beta_k \eta_k
\end{equation*}
>$$
>
> where $\lbrace \alpha_k\rbrace_{k=1}^{\infty},\; \lbrace \beta_k\rbrace_{k=1}^{\infty},\; \lbrace \eta_k\rbrace_{k=1}^{\infty}$ are stochastic sequences with \\( \alpha_k, \beta_k \ge 0 \\) for all \\(k\\). Then, \\(\delta_k \to 0\\) almost surely if the following conditions are satisfied:
> 1. \\( \sum_{k=1}^{\infty} \alpha_k = \infty \\), \\( \sum_{k=1}^{\infty} \alpha_k^2 < \infty \\), and \\( \sum_{k=1}^{\infty} \beta_k^2 < \infty \\).
> 2. \\( \mathbb{E}[\eta_k \| \mathcal{H}_k] = 0 \\) and \\( \mathbb{E}[\eta_k^2 \| \mathcal{H}_k] \le C < \infty \\) for some constant \\(C\\) almost surely, and \\( \mathcal{H}_k\\) is the history of the process up to iteration \\(k\\).

These conditions have similar interpretations as the conditions in the Robbins-Monro theorem. Note that in Dvoretzky's theorem, \\(\alpha_k, \beta_k, \eta_k\\) are stochastic sequences, which means that they can vary randomly at each iteration. This is in contrast to the Robbins-Monro theorem, where \\(\alpha_k\\) is a deterministic sequence. Now, we can discuss a more general form of Dvoretzky's convergence theorem which incorporates multiple variables and is useful in proving the convergence of Q-learning algorithms.

> **Theorem (Generalized Dvoretzky's Convergence Theorem)**: Consider a stochastic process
>
>$$
\begin{equation*}
    \delta_{k+1}(s) = (1-\alpha_k(s)) \delta_k(s) + \beta_k(s) \eta_k(s)
\end{equation*}
>$$
>
> it holds that \\(\delta_k(s) \to 0\\) almost surely for all \\(s \in \mathcal{S}\\) if the following conditions are satisfied for all \\(s \in \mathcal{S}\\):
> 1. \\( \sum_{k=1}^{\infty} \alpha_k(s) = \infty \\), \\( \sum_{k=1}^{\infty} \alpha_k(s)^2 < \infty \\), \\( \sum_{k=1}^{\infty} \beta_k(s)^2 < \infty \\), and \\( \mathbb{E}[\beta_k(s) \| \mathcal{H}_k] \le \mathbb{E}[\alpha_k(s) \| \mathcal{H}_k] \\) uniformly almost surely.
> 2. \\({\norm{\mathbb{E}\\left[\eta_k(s) \mid \mathcal{H}_k\\right]}}_{\\infty} \\le\\), for \\( \gamma \in (0, 1) \\)
> 3. \\( \text{Var}[\eta_k(s) \| \mathcal{H}_k] \le C(1+\norm{\delta_k(s)}_\infty)^2 \\) for some constant \\(C\\).