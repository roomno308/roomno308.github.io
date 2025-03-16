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

> A policy \\(\pi^*\\) is optimal if \\(\pi^*(s) \geq \pi(s) \quad \forall s \in \mathcal{S}\\) and for any policy other policy \\(\pi\\). The state values of an optimal policy are called *optimal state values* and are denoted by \\(v_*(s)\\).