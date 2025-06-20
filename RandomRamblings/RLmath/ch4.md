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

The policy iteration algorithm is another method to find the optimal policy and value function. It consists of two main steps:

1. **Policy Evaluation**: This step computes the value function for a given policy. It does so by solving the Bellman equation: \\( v_{\pi_k} = r_{\pi_k} + \gamma P_{\pi_k} v_{\pi_k} \\).

2. **Policy Improvement**: Once the value function is computed, this step improves the policy by solving the equation: \\( \pi_{k+1} = \arg\max_\pi \left( r_\pi + \gamma P_\pi v_{\pi_k} \right) \\).


With the above description of the algorithm, following questions arise:

1. In the policy evaluation step, how do we compute the value function for a given policy?

2. Why is the policy improvement step guaranteed to improve the policy?

3. Can the algorithm converge to the optimal policy and value function?

Let us answer these questions one by one.

### In the policy evaluation step, how do we compute the value function for a given policy?

To compute the value function, we need to solve the Bellman evaluation equation:

$$
\begin{equation*}
\begin{split}
    v_{\pi_k} &= r_{\pi_k} + \gamma P_{\pi_k} v_{\pi_k} \\
\end{split}
\end{equation*}
$$

As discussed in [Chapter 2](ch2.html#bellman-equation-for-state-values), this equation can either be solved in closed form or iteratively. The closed form solution is not usually practical, so we solve this equation iteratively using the following update rule:

$$
\begin{equation*}
\begin{split}
    v^{(j+1)}_{\pi_k} = r_{\pi_k} + \gamma P_{\pi_k} v^{(j)}_{\pi_k}
\end{split}
\end{equation*}
$$

> **Note**: The policy iteration is an iterative algorithm that contains another iterative algorithm embedded in it. The outer loop is the policy iteration loop (denoted by \\(k\\)), and the inner loop is the policy evaluation loop (denoted by \\(j\\)). It is also noteworthy that the embeddeed loop requires an infinite number of steps to converge to the true value function, but in practice, we can stop after a finite number of steps. (more on this in truncated policy iteration)

### Why is the policy improvement step guaranteed to improve the policy?

> **Lemma** (Policy Improvement): If \\(v_{\pi_{k+1}} = \arg\max_\pi \left( r_\pi + \gamma P_\pi v_{\pi_k} \right) \\), then \\(v_{\pi_{k+1}} \geq v_{\pi_k}\\) for all states \\(s\\).

> **Proof**: We know that:
>
>$$
\begin{equation*}
\begin{split}
    v_{\pi_{k+1}} &= r_{\pi_{k+1}} + \gamma P_{\pi_{k+1}} v_{\pi_k}
    \\
    v_{\pi_k} &= r_{\pi_k} + \gamma P_{\pi_k} v_{\pi_k}
\end{split}
\end{equation*}
>$$
>
> Since \\(v_{\pi_{k+1}}\\) is the maximum value function over all policies, we have:
>
>$$
\begin{equation*}
\begin{split}
    r_{\pi_{k+1}} + \gamma P_{\pi_{k+1}} v_{\pi_k} &\geq r_{\pi_k} + \gamma P_{\pi_k} v_{\pi_k} \\
    \Rightarrow v_{\pi_{k+1}} + \gamma P_{\pi_{k+1}} (v_{\pi_k} - v_{\pi_{k+1}}) &\geq v_{\pi_k} \\
    \Rightarrow (v_{\pi_{k}} - v_{\pi_{k+1}} &\leq \gamma P_{\pi_{k+1}} (v_{\pi_k} - v_{\pi_{k+1}}) \\
\end{split}
\end{equation*}
>$$

Since \\(P_{\pi_{k+1}}\\) is a stochastic matrix, we know that \\( (v_{\pi_{k}} - v_{\pi_{k+1}}) \leq \gamma P_{\pi_{k+1}} (v_{\pi_k} - v_{\pi_{k+1}}) \leq \gamma^2 P_{\pi_{k+1}}^2 (v_{\pi_k} - v_{\pi_{k+1}}) \leq \ldots \leq \lim_{n \to \infty} \gamma^n P_{\pi_{k+1}}^n (v_{\pi_k} - v_{\pi_{k+1}}) = 0\\).

Thus, we have \\(v_{\pi_{k+1}} \geq v_{\pi_k}\\) for all states \\(s\\).

### Convergence of Policy Iteration
The policy iteration algorithm generates a sequence of policies \\(\pi_0, \pi_1, \ldots\\) and value functions \\(v_{\pi_0}, v_{\pi_1}, \ldots\\). We already know from previous Lemma that:

$$
\begin{equation*}
\begin{split}
    v_{\pi_{0}} &\leq v_{\pi_{1}} \leq v_{\pi_{2}} \leq \ldots
\end{split}
\end{equation*}
$$

Also, we know that all these value functions are bounded by the maximum possible return \\(v^{\*}\\), which is finite. Since the sequence of value functions, \\(v_k\\), is monotonically increasing and bounded by a finite value, it follows from the [Monotone Convergence Theorem](https://en.wikipedia.org/wiki/Monotone_convergence_theorem) that the sequence will converge to a constant value denoted by \\(v_\infty\\), when \\( k \to \infty \\). We need to show that this value is the optimal value function, i.e., \\(v_\infty = v^*\\).

> **Theorem** (Convergence of Policy Iteration): The state value sequence \\( \{v_k\}_{k=0}^{\infty} \\) converges to the optimal state value function \\(v^*\\). As a result, the corresponding policy sequence converges to the optimal policy.

> **Proof**: The proof is left as an exercise (refer the book)