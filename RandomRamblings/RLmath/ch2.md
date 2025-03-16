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

$$
\begin{equation}
    \begin{split}
        v_{\pi}(s) & = \mathbb{E}_{\pi}[G_t | S_t = s] \\
        & = \mathbb{E}_{\pi}[R_{t+1} + \gamma G_{t+1} | S_t = s] \\
        & = \mathbb{E}_{\pi}[R_{t+1} | S_t = s] + \gamma \mathbb{E}_{\pi}[G_{t+1} | S_t = s]
    \end{split}
\end{equation}
\label{eq:valuereturn}
$$

The first term in the RHS of Equation $\eqref{eq:valuereturn}$ is the expected reward from state \\(s\\) under policy \\(\pi\\), and can be expressed as:

$$
    \begin{equation}
        \begin{split}
            \mathbb{E}[R_{t+1} | S_t = s] & = \sum_{a \in \mathcal{A}} \pi(a|s) \mathbb{E}[R_{t+1} | S_t = s, A_t = a] \\

            & = \sum_{a \in \mathcal{A}} \pi(a|s) \sum_{r \in \mathcal{R}} p(r|s, a) r
        \end{split}
    \end{equation}
$$

The second term in the RHS of Equation $\eqref{eq:valuereturn}$ is the expected future return. This can be expressed as:

$$
    \begin{equation}
        \begin{split}
            \mathbb{E}_{\pi}[G_{t+1} | S_t = s] & = \sum_{s' \in \mathcal{S}} \mathbb{E}_{\pi}[G_{t+1} | S_{t+1} = s'] p(s' | s) \\
            & = \sum_{s' \in \mathcal{S}} v_{\pi}(s') p(s' | s) \\
            & = \sum_{s' \in \mathcal{S}} v_{\pi}(s') \sum_{a \in \mathcal{A}} \pi(a|s) p(s' | s, a) \\
            & = \sum_{a \in \mathcal{A}} \pi(a|s) \sum_{s' \in \mathcal{S}} v_{\pi}(s') p(s' | s, a)
        \end{split}
    \end{equation}
$$

Substituting the above two equations back into Equation $\eqref{eq:valuereturn}$, we get the Bellman equation for the state value function:

$$
    \begin{equation}
        \begin{split}
            v_{\pi}(s) & = \sum_{a \in \mathcal{A}} \pi(a|s) \sum_{r \in \mathcal{R}} p(r|s, a) r + \gamma \sum_{a \in \mathcal{A}} \pi(a|s) \sum_{s' \in \mathcal{S}} v_{\pi}(s') p(s' | s, a) \\

            & = \sum_{a \in \mathcal{A}} \pi(a|s) \left( \sum_{r \in \mathcal{R}} p(r|s, a) r + \gamma \sum_{s' \in \mathcal{S}} v_{\pi}(s') p(s' | s, a) \right) \\

            & = \sum_{a \in \mathcal{A}} \pi(a|s) \sum_{s', r} p(s', r | s, a) \left( r + \gamma v_{\pi}(s') \right)
        \end{split}
    \end{equation}
    \label{eq:bellmanvalue}
$$

Equation $\eqref{eq:bellmanvalue}$ is the Bellman equation for the state value function. Note that:

1. Bellman equation is a set of linear equations that express the relationship between the values of states rather than a single equation.

2. Solving the Bellman equation for a given policy \\(\pi\\) gives the state value function \\(v_{\pi}(s)\\) and is a *policy evaluation process*.

3. $p(r|s, a)$ and $p(s' | s, a)$ represent the system model. 

## Matrix-Vector Form of Bellman Equation