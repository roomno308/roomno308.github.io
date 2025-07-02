---
layout: post
title: Chapter 2 - State Values and Bellman Equation
author: Chaitanya
---

[Notes](notes.html) | [Next Chapter](ch3.html)

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

3. \\(p(r\|s, a)\\) and \\(p(s' \| s, a)\\) represent the system model. 

## Matrix-Vector Form of Bellman Equation

The Bellman equation can be expressed in matrix-vector form as:

$$
    \begin{equation}
        v_{\pi} = r^{\pi} + \gamma P^{\pi} v_{\pi}
    \end{equation}
$$

or simply:

$$
    \begin{equation}
        v = r + \gamma P v
    \end{equation}
$$

where:

- \\(v_{\pi}\\) is the state value function vector under policy \\(\pi\\).
- \\(r^{\pi}\\) is the reward vector under policy \\(\pi\\).
- \\(P^{\pi}\\) is the transition probability matrix under policy \\(\pi\\).

### Solving Bellman Equation
#### Closed Form Solution

Since the Bellman equation is a linear equation, a closed form solution can be easily obtained as:

$$
    v = (I - \gamma P)^{-1} r
$$

Some properties of the matrix \\(I - \gamma P\\) are:

1. $(I - \gamma P)$ is invertible. Proof with Gershgorin circle theorem??

2. \\((I - \gamma P)^{-1} \ge I\\): Every element of \\((I - \gamma P)^{-1}\\) is greater than or equal to 0. This is because $P$ has non-negative entries and by using the Neumann series expansion of the inverse matrix, we get: \\((I - \gamma P)^{-1} = I + \gamma P + \gamma^2 P^2 + \ldots \ge 0 \\).

3. For any vector $r \ge 0$,  \\((I - \gamma P)^{-1} r \ge r \ge 0\\).

#### Iterative Solution

Consider the iterative solution to the Bellman equation:

$$
    v_{k+1} = r + \gamma P v_k
$$

This algorithm produces a series of values \\(v_0, v_1, v_2, \ldots\\) starting from some initial guess. The algorithm converges to the true value function \\(v_{\pi}\\) as \\(k \rightarrow \infty\\). Proof:

We define \\( \delta_k = v_{k} - v_{\pi}\\) and show that \\( \delta_k \rightarrow 0\\) as \\(k \rightarrow \infty\\).

(\\(\delta_{k+1} = v_{k+1} - v_{\pi} \\))

$$
    \begin{equation}
        \begin{split}
            \delta_{k+1} + v_{\pi} & = r + \gamma P(\delta_k + v_{\pi}) \\
            \delta_{k+1} & = \gamma P \delta_k
        \end{split}
    \end{equation}
$$

Hence, \\( \delta_{k+1} = \gamma P \delta_k = \gamma^2 P^2 \delta_{k-1} = \ldots = \gamma^{k+1} P^{k+1} \delta_0\\). Since \\(P\\) is a transition probability matrix and \\(\gamma \in (0, 1)\\), \\(\gamma^k P^k \rightarrow 0\\) as \\(k \rightarrow \infty\\). Hence, \\( \delta_k \rightarrow 0\\) as \\(k \rightarrow \infty\\).

## State-Action Values

The state-action value function \\(q_{\pi}(s, a)\\) is the expected return from state \\(s\\) and action \\(a\\) under policy \\(\pi\\). Note that the policy is followed after taking the action \\(a\\). The state-action value function can be expressed as:

$$
    q_{\pi}(s, a) = \mathbb{E}_{\pi}[G_t | S_t = s, A_t = a]
$$

### Relationship between State and State-Action Values

<ul>
<li> From conditional expectation, we have:

$$
    v_{\pi}(s) = \sum_{a \in \mathcal{A}} \pi(a|s) q_{\pi}(s, a)
$$
</li>

<li> Furthermore, since the state value can be written as:

$$
    v_{\pi}(s) = \sum_{a \in \mathcal{A}} \pi(a|s) \left( \sum_{r \in \mathcal{R}} p(r|s, a) r + \gamma \sum_{s' \in \mathcal{S}} v_{\pi}(s') p(s' | s, a) \right)
$$

we can write the state-action value function as:

$$
    q_{\pi}(s, a) = \sum_{r \in \mathcal{R}} p(r|s, a) r + \gamma \sum_{s' \in \mathcal{S}} v_{\pi}(s') p(s' | s, a)
$$
</li>
</ul>

### Bellman Equation for State-Action Values

From the equation above:

$$
    \begin{equation}
        \begin{split}
            q_{\pi}(s, a) &= \sum_{r \in \mathcal{R}} p(r|s, a) r + \gamma \sum_{s' \in \mathcal{S}} v_{\pi}(s') p(s' | s, a) \\

            &= \sum_{r \in \mathcal{R}} p(r|s, a) r + \gamma \sum_{s' \in \mathcal{S}} p(s' | s, a) \sum_{a' \in \mathcal{A}} \pi(a'|s') q_{\pi}(s', a')
        \end{split}
    \end{equation}
$$

This is the Bellman equation for the state-action value function. The Bellman equation for the state-action value function can be expressed in matrix-vector form as:

$$
    q_{\pi} = \widetilde{r} + \gamma P \Pi q_{\pi}
$$

Here:

- \\(q_{\pi}\\) is the state-action value function vector under policy \\(\pi\\).

- \\(\widetilde{r}\\) is the immediate reward vector and **does not** depend on the policy (because we know that we are taking the action $a$ immediately).

- \\(P\\) is the transition probability matrix which, unlike the state value Bellman equation, **does not** depend on the policy.