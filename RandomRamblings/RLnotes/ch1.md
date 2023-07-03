---
layout: post
title: Introduction
author: 308
---

## Reinforcement Learning

*"Reinforcement Learning is learning what to do - how to map situations to actions - so as to maximize a numerical reward signal. The learner is not told which actions to take, but instead must discover which actions yield the most reward by trying them."*

2 main characteristics:
- Trial and error search 
- delayed reward

there is a tradeoff between exploration and exploitation: the agent is supposed to take actions that maximize the rewards, but in order to discover the best actions, it has to explore the environment. Neither exploration nor exploitation can be pursued exclusively without failing at the task.

## Elements of Reinforcement Learning

1. Policy: A policy defines the learning agent's way of behaving at a given time. It is a mapping from states to actions.
2. Reward signal: A reward signal defines goal in RL problem. On each time step, the env sends to the agent a single number called *reward*. Agent's sole objective is to maximize the reward it gets over a long run.
3. Value function: Value of a state is the total amount of reward an agent can expect to accumulate over the future, starting from that state. 
4. Model of the environment: model is something that mimics/approximates the working of the environment. For example, given a state and action, the model might predict the next state and the next reward. RL algorithms that use models and planning are called *model-based* methods, as opposed to simpler *model-free* methods which use trial and error.

## Limitations and Scope
Reinforcement learning relies heavily on the concept of state -- as input to the policy and value function. The state is a signal of "how the environment is at a particular time". Most of the RL methods *we consider in the book* are structured around estimating value functions, but RL problems can be solved in other ways as well. For example, evolutionary algorithms or simulated annealing can be used to find good policies without estimating value functions.
These methods can be advantageous when the seatch space of the policies is sufficiently small, or can be structured so that the good policies are easy to find. Moreover, they can be advantageous when the agent can't sense the complete state of the environment or the state is misperceived.