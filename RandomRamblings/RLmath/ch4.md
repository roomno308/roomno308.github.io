---
layout: post
title: Chapter 4 - Value Iteration and Policy Iteration
author: Chaitanya
---

## Value Iteration

Value iteration is exactly the algorithm suggested by the Bellman Optimality Equation:

$$
\begin{equation*}
\begin{split}
    v_{k+1} &= max_{\pi \in \Pi} \left( r_{\pi} + \gamma P_{\pi} v_k \right) \\
\end{split}
\end{equation*}
$$

It is guaranteed to converge to the optimal state-value and optimal policy by the [Contraction Mapping Theorem](ch3.html#contraction-mapping-theorem).