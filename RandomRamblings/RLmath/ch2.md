---
layout: post
title: Chapter 2 - State Values and Bellman Equation
author: Chaitanya
---

## State Values

Consider a sequence of timesteps \\(t = 0, 1, 2, 3, \ldots\\). At each timestep \\(t\\), the agent is in a state \\(S_t\\) and takes an action \\(A_t\\) following a policy \\(\pi\\). The agent receives a reward \\(R_{t+1}\\) and transitions to a new state \\(S_{t+1}\\). Rhis can be represented as:

$$
    S_t \xrightarrow{A_t} R_{t+1}, S_{t+1}
$$

\\(S_t, A_t, R_{t+1}, S_{t+1}\\) are all random variables belonging to their respective spaces in the MDP. Then, starting from state \\(S_t\\),  the state-action-reward trajectory is:

$$
    S_t \xrightarrow{A_t} R_{t+1}, S_{t+1} \xrightarrow{A_{t+1}} R_{t+2}, S_{t+2} \xrightarrow{A_{t+2}} R_{t+3}, S_{t+3} \ldots
$$

The return \\(G_t\\) is the total discounted reward from timestep \\(t\\) onwards:

$$
    G_t = R_{t+1} + \gamma R_{t+2} + \gamma^2 R_{t+3} + \ldots = \sum_{k=0}^{\infty} \gamma^k R_{t+k+1}
$$

where \\(\gamma \in (0, 1)\\) is the discount factor. Note that the return is a random variable as it depends on the random variables \\(R_{t+1}, R_{t+2}, \ldots\\). The *state value* function \\(v_{\pi}(s)\\) is the expected return from state \\(s\\) under policy \\(\pi\\):

$$
    v_{\pi}(s) = \mathbb{E}_{\pi}[G_t | S_t = s]
$$


## Bellman Equation

In a nutshell, the Bellman equation is a set of linear equaitons that express the relationship between the values of states. First, we can see that:

$$
    G_t = R_{t+1} + \gamma R_{t+2} + \gamma^2 R_{t+3} \ldots = R_{t+1} + \gamma G_{t+1}
$$

Then, the state value function can be expressed as: $trial$

\\begin{equation}
    v_{\pi}(s) = \mathbb{E}_{\pi}[G_t | S_t = s] = \mathbb{E}_{\pi}[R_{t+1} + \gamma G_{t+1} | S_t = s] = \sum_{a} \pi(a|s) \sum_{s', r} p(s', r | s, a) [r + \gamma v_{\pi}(s')]
\\end{equation}



