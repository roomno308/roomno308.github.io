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

It is guaranteed to converge to the optimal state-value and optimal policy by the [Contraction Mapping Theorem](ch3.html#contraction-mapping-theorem). Each iteration, value iteration performs two steps:

1. **Policy Update**: It aims to find the optimal policy that can solve: \\( \pi_{k+1} = \arg\max_\pi \left( r_\pi + \gamma P_\pi v_k \right) \\)

2. **Value Update**: It updates the value function using: \\( v_{k+1} = r_{\pi_{k+1}} + \gamma P_{\pi_{k+1}} v_k \\)

Note that value iteration is a dynamic programming algorithm that does not require a model of the environment. Also, it is noteworthy that the value estimate $v_k$ is not the true value function. Even though it converges to the true value function, it is not guaranteed to satisfy the Bellman Optimality Equation for any policy at any iteration.

## Policy Iteration
