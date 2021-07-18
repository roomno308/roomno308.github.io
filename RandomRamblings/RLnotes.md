---
layout: post
title: RL Notes 
author: Yashas, Tanmay, Chaitanya
---

Discussions on Reinforcement Learning based on [RL lecture series](https://www.youtube.com/watch?v=ISk80iLhdfU&list=PLqYmG7hTraZBKeNJ-JE_eyJHZ7XgBoAyb)

## Lecture 2

### Action Value
For a single state environment, action value is defined as the expected reward recieved for performing the action. Formally,

\\[q(a) = \mathbb{E}(R_t \mid A_t = a)\\]

Therefore, the sample average will become:

\\[Q_t(a) = \frac{\sum_{n = 1}^t R_n \mathcal{I}(A_n = a)}{\sum_{n = 1}^t \mathcal{I}(A_n = a)}\\]

Where, \\(\mathcal{I}\\) works as a delta function giving 1 if \\(A_n = a\\), else, 0. 

### Why is exploration important?

Let's say that we need to develop an algorithm that maximizes the reward throughout the lifetime of an agent. Someone might argue that always taking the action with highest sample average of the action value (\\(Q\\)) might be a good idea (*greedy algorithm*). Let us tell you beforehand, it isn't, and we will try to establish why it isn't the best idea to go greedy in the rest of this subsection.

![](https://i.imgur.com/CfFOH5p.png)

Let's say that the mouse wants to maximize its reward (cheeze) and minimize the pain (shock). Also, let's say that the probability of getting cheeze after pulling the lever is such as:

\\[p(cheeze \mid black\ lever) = 0.8\\] \\[p(cheeze \mid white\ lever) = 0.2\\]

Also, \\(p(shock \mid lever) = 1 - p(cheeze \mid lever)\\)

Obviously, the optimal strategy here is to spam the black lever. But, let's say our "greedy" mouse tries both the levers in the first two episodes, gets shocked after pulling the black lever, and gets reward for pulling the white lever. Now the sample action value becomes:

\\[Q_2(Black\ lever) = -1\\] \\[Q_2(White\ lever) = 1\\]

![](https://i.imgur.com/h75zyio.png)

Since the sample value of the white lever is higher, it keeps on choosing the white lever again and again and keeps getting shocked again and again. Since the sample value of black lever is -1, the sample value of the white lever will never go below it, and hence, the greedy mouse will never be able to select the "optimal" black lever.

So what went wrong here? We knew that the sample value of white lever is greater than that of the black lever, but since that value came from one sample of each action, we weren'e very confident if the sample value matches the actual action value...

In order to gain **confidence** in some action, exploration is required along with exploitation.