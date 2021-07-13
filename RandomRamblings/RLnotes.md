---
layout: post
title: RL Notes 
author: Yashas, Tanmay, Chaitanya
---

Discussions on Reinforcement Learning based on [RL lecture series](https://www.youtube.com/watch?v=ISk80iLhdfU&list=PLqYmG7hTraZBKeNJ-JE_eyJHZ7XgBoAyb)

### Lecture 2

#### Action Value
For a single state environment, action value is defined as the expected reward recieved for performing the action. Formally,

\\[q(a) = \mathbb{E}(R_t \mid A_t = a)\\]

Therefore, the sample average will become:

\\[Q_t(a) = \frac{\sum_{n = 1}^t R_n \mathcal{I}(A_n = a)}{\sum_{n = 1}^t \mathcal{I}(A_n = a)}\\]

Where, \\(\mathcal{I}\\) works as a delta function giving 1 if \\(A_n = a\\), else, 0. 

#### Why is exploration important?

![](https://i.imgur.com/CfFOH5p.png)

