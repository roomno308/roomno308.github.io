---
layout: post
title: Chapter 5 - Monte Carlo Methods
author: Chaitanya
---

Till now, we have only discussed methods that require a model of the environment. These methods are often referred to as **model-based methods**. However, in many real-world scenarios, we may not have access to the model of the environment. In such cases, we can use **model-free methods** that do not require a model of the environment.

In this chapter, we will discuss once such method to convert the model-based methods to model-free methods. This method is called **Monte Carlo Methods**.

## Mean Estimation

Refer to the book chapter to read more about mean estimation.

## Monte Carlo Methods

Monte Carlo methods are a class of algorithms that rely on repeated random sampling to obtain numerical results. In the context of reinforcement learning, Monte Carlo methods can be used to estimate the value function of a policy by averaging the returns from multiple episodes.

### A Simple Monte Carlo Algorithm

To construct a simple Monte Carlo algorithm, we can modify the policy iteration algorithm to use Monte Carlo methods for policy evaluation. Here, we use the state-action value function instead of the state value function because:
1. It is easier to estimate the state-action value function from sampled trajectories.
2. Calculating the policy in the policy improvement step is easier with the state-action value function, because we don't have the model for transition probabilities.

Therefore, the simple MC algorithm can be described as follows:

1. *Policy evaluation*: In this step we estimate the state-action value \\( q_{\pi_k}(s,a) \\) for all state-action pairs \\( (s,a) \\). Specifically, for every \\((s,a)\\), we sample "sufficiently many" episodes and approximate the true state-action value \\( q_{\pi_k}(s,a) \\) using the average of the returns \\( q_k(s,a) \\) from these episodes.

2. *Policy improvement*: In this step, we improve the policy by choosing the action that maximizes the state-action value function: \\( \pi_{k+1}(s) = \arg\max_a q_k(s,a) \\).

This algorithm, although good for understanding the concept of Monte Carlo methods, is not practical for large state spaces. This is because it requires sampling a large number of episodes to estimate the state-action value function accurately and has a very low sample efficiency.

### First set of Improvements

## Sample Efficiency

In the basic Monte Carlo algorithm, we sample episodes starting from each state-action pair to estimate the state-action value function. But, during every episode, the agent may visit different states and perform different actions in them. This implies that we can use the information from the episodes to update the state-action value function for all the states and actions visited in that episode instead of just the state-action pair we started from. For example for a sample episode starting from state \\(s_1\\) and action \\(a_2\\) (the state and action subscripts denote the state and action indices and not the time step):

$$
\begin{equation*}
\begin{split}
    s_1 \xrightarrow{a_2} s_2 \xrightarrow{a_4} s_1 \xrightarrow{a_2} s_2 \xrightarrow{a_3} s_5 \xrightarrow{a_1} & \cdots \;  \text{[original episode]} \\
    s_2 \xrightarrow{a_4} s_1 \xrightarrow{a_2} s_2 \xrightarrow{a_3} s_5 \xrightarrow{a_1} & \cdots \;  \text{[subepisode starting from } (s_2,a_4) \text{]} \\
    s_1 \xrightarrow{a_2} s_2 \xrightarrow{a_3} s_5 \xrightarrow{a_1} & \cdots \;  \text{[subepisode starting from } (s_1,a_2) \text{]} \\
    s_2 \xrightarrow{a_3} s_5 \xrightarrow{a_1} & \cdots \;  \text{[subepisode starting from }(s_2,a_3) \text{]} \\
    s_5 \xrightarrow{a_1} & \cdots \;  \text{[subepisode starting from }(s_5,a_1) \text{]}
\end{split}
\end{equation*}
$$

In the above example, we can see that the original episode can be split into multiple subepisodes starting from different state-action pairs. We can use these subepisodes to update the state-action value function for all the state-action pairs visited in that episode. If we use only the first visit of a state-action pair in an episode, it is called **first-visit MC**. If we use all the visits of a state-action pair in an episode, it is called **every-visit MC**.

> Note that even though the every visit MC is more sample efficient than the first visit MC, the samples it uses are not independent because the second visit is just a subset of the first visit and the visits are not independent. Nevertheless, this correlation might not be strong in practice if each visit is far apart in time.

### Policy Update

Our basic algorithm collected a bunch of trajectories and then used the average to update the state-action value function. This can be quite slow and impractical, therefore, instead of collecting a bunch of episodes, we can update the state-action value function after every episode using running average. Even though the return of a single episode is not an accurate estimate of the corresponding action value, this is still a good strategy and falls into the scope of **generalized policy iteration** methods (Last chapter of the book).