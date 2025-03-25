---
layout: post
title: Chapter 3 - Optimal Sate Values and Bellman Optimality Equation
author: Chaitanya
---

## Optimal State Values and Optimal Policies

Since in RL, our goal is to find an optimal policy, we need to define what an optimal policy is. Given two policies \\(\pi_1\\) and \\(\pi_2\\), we say that \\(\pi_1\\) is better than \\(\pi_2\\) if the state value of \\(\pi_1\\) is greater than or equal to that of \\(\pi_2\\) for all states. Formally, \\(\pi_1 \geq \pi_2\\) if:

$$
v_{\pi_1}(s) \geq v_{\pi_2}(s) \quad \forall s \in \mathcal{S}
$$

If a policy is better than all other policies, it is called an *optimal policy*. Formally:

> A policy \\(\pi^\*\\) is optimal if \\(\pi^\*(s) \geq \pi(s) \quad \forall s \in \mathcal{S}\\) and for any policy other policy \\(\pi\\). The state values of an optimal policy are called *optimal state values* and are denoted by \\(v_*(s)\\).

In this chapter, we will answer the following questions about the optimal policy:

- Existence
- Uniqueness
- Stochasticity: Is the optimal policy deterministic or stochastic?
- Algorithm: How can we find the optimal policy? (maybe in the next chapter?)

## Bellman Optimality Equation

The bellman Optimality Equation is given by:

$$
\begin{equation}
    \begin{split}
        v(s) &= max_{\pi(s) \in \Pi(s)} \sum_{a \in \mathcal{A}} \pi(a|s) \left( \sum_{r \in \mathcal{R}} p(r|s, a) r + \gamma \sum_{s' \in \mathcal{S}} v(s') p(s' | s, a) \right) \\

        &= max_{\pi(s) \in \Pi(s)} \sum_{a \in \mathcal{A}} \pi(a|s) q(s, a)
    \end{split}
\end{equation}
$$

The optimality equation has two unknown variables, the state value function \\(v(s)\\) and the policy \\(\pi(s)\\). It might be non trivial to understand how to solve two unknowns with one equation. To understand this better, take an example where we have to maximize the weighted sum of some numbers:

$$
max_{w} \sum_{i} w_i x_i
$$

Subject to the constraint that the weights sum to 1. \\(w_i \geq 0\\) for all \\(i\\) and \\(\sum_{i} w_i = 1\\). To maximize the sum, we need to assign the weights to the numbers such that the sum is maximized. The solution to such an equation would be to \\(w_i = 1\\) for the number with the maximum value and \\(w_i = 0\\) for all other numbers. This is the same as the Bellman Optimality Equation. The policy \\(\pi(a\|s)\\) is 1 for the action that maximizes the state value and 0 for all other actions. Therefore:

$$
\begin{equation}
    \begin{split}
        v(s) &= max_{\pi(s) \in \Pi(s)} \sum_{a \in \mathcal{A}} \pi(a|s) q(s, a) \\
        &\le max_{a \in \mathcal{A}} q(s, a)
    \end{split}
\end{equation}
$$

The equality is achieved when:

$$
\pi(a|s) = \begin{cases}
    1 & \text{if } a = \arg\max_{a \in \mathcal{A}} q(s, a) \\
    0 & \text{otherwise}
\end{cases}
$$

### Matrix Vector Form

As before, the Bellman Optimality Equation can be expressed in matrix-vector form as:

$$
f(v) = \max_{\pi \in \Pi} \left( r_{\pi} + \gamma P_{\pi} v \right)
$$

Then, BOE can be written as:

$$
    \begin{equation}
    \begin{split}
        v = f(v)
    \end{split}
    \end{equation}
    \label{eq:MatrixBOE}
$$

## Contraction Mapping

A function $f$ is a contraction mapping if:

$$
\| f(x_1) - f(x_2) \| \leq \gamma \| x_1 - x_2 \|
$$

#### Contraction Mapping Theorem

> For any any equation of the form \\(x = f(x)\\), if \\(x\\) and \\(f(x)\\) are real vectors, and \\(f\\) is a contraction mapping then the following properties hold:
>
> - _Existence_: There exists a fixed point \\(x^\*\\) satisfying \\(f(x^\*) = x^\*\\).
> - _Uniqueness_: The fixed point is unique.
> - _Algorithm_: The fixed point can be found by iterating the equation \\(x_{k+1} = f(x_k)\\) starting from any initial vector \\(x_0\\). \\(x_k \to x^\*\\) as \\(k \to \infty\\). Moreover, the rate of convergence is exponential.

**Proof of Contractive Mapping Theorem**

> This proof is left as an exercise to the reader. (available in the book)


#### BOE as a Contraction Mapping

We want to show that the function $f(v)$ in equaiton \eqref{eq:MatrixBOE} is a contraction mapping. 

> **Theorem** _(Contraction property of $f(v)$)_: The function $f(v)$ on the right hand side of the equation \eqref{eq:MatrixBOE} is a contraction mapping. Formally, for any two vectors $v_1$ and $v_2$:
>
>$$
\| f(v_1) - f(v_2) \| \leq \gamma \| v_1 - v_2 \|
>$$

> **Proof**:
>
> let $v_1$ and $v_2$ be any two vectors and $\pi_1 = \arg\max_{\pi \in \Pi} \left( r_{\pi} + \gamma P_{\pi} v_1 \right)$ and $\pi_2 = \arg\max_{\pi \in \Pi} \left( r_{\pi} + \gamma P_{\pi} v_2 \right)$. Then:
>
>$$
\begin{equation}
\begin{split}
    f(v_1) & = \max_{\pi \in \Pi} \left( r_{\pi} + \gamma P_{\pi} v_1 \right) \\
    & = r_{\pi_1} + \gamma P_{\pi_1} v_1 \\
    & \geq r_{\pi_2} + \gamma P_{\pi_2} v_1 \\
    f(v_2) & = \max_{\pi \in \Pi} \left( r_{\pi} + \gamma P_{\pi} v_2 \right) \\
    & = r_{\pi_2} + \gamma P_{\pi_2} v_2 \\
    & \geq r_{\pi_1} + \gamma P_{\pi_1} v_2
\end{split}
\end{equation}
>$$
>
> Therefore:
>
>$$
\begin{equation}
\begin{split}
    f(v_1) - f(v_2)  & = r_{\pi_1} + \gamma P_{\pi_1} v_1 - r_{\pi_2} - \gamma P_{\pi_2} v_2 \\
    & \leq \gamma P_{\pi_1} (v_1 - v_2)
\end{split}
\end{equation}
>$$
>
> Similarly, $f(v_2) - f(v_1) \leq \gamma P_{\pi_2} (v_2 - v_1)$.
>
> Let us define $z \doteq \max\{\| \gamma P_{\pi_1} (v_1 - v_2) \|, \| \gamma P_{\pi_2} (v_2 - v_1) \|\}$. Then:
>
>$$
\begin{equation}
\begin{split}
    \| f(v_1) - f(v_2) \| \leq z
\end{split}
\end{equation}
>$$
>
> Hence, It follows that:
>
>$$
\begin{equation}
\begin{split}
    \| f(v_1) - f(v_2) \|_{\infty} \leq \| z \|_{\infty} 
\end{split}
\end{equation}
>$$
>
> Now, since $P$  is a stochastic matrix with each row summing to 1, we have (for any row $p_i$):
>
>$$
\begin{equation}
\begin{split}
    \| p_i^T (v_1 - v_2) \| \leq p_i^T \| v_1 - v_2 \| \leq \| v_1 - v_2 \|_{\infty}
\end{split}
\end{equation}
>$$
>
> Therefore, $\| z \|_{\infty} \leq \gamma \| v_1 - v_2 \|_{\infty}$. Hence:
>
>$$
\begin{equation}
\begin{split}
    \| f(v_1) - f(v_2) \| \leq \gamma \| v_1 - v_2 \|
\end{split}
\end{equation}
>$$
>
> This completes the proof.