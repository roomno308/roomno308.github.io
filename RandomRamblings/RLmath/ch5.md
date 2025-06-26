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