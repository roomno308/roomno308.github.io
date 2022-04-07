---
layout: post
title: RL Notes 
author: Chaitanya, Tanmay, Yashas
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

So what went wrong here? We knew that the sample value of white lever is greater than that of the black lever, but since that value came from one sample of each action, we weren't very confident if the sample value matches the actual action value...

In order to gain **confidence** in some action, exploration is required along with exploitation.

### Trading off exploration and exploitation

for trading off between exploration and exploitation based on our confidence of an action, we define a few terms:

**Optimal Value** is defined as the maximum true action value among all the actions. Formally,

\\[v_* = \underset{a \in \mathcal{A}}{\operatorname{max}} \: q(a) = \underset{a \in \mathcal{A}}{\operatorname{max}} \: \mathbb{E}(R_t \mid A_t = a)\\]

**Regret** for a step is defined as the opportuinity loss in that step, i.e. the difference between the optimal value and value of the action performed. Therefore, total regret till time \\(t\\) becomes the sum of all step regrets,

\\[L_t = \sum_{i=1}^{t}(v_* - q(a_i))\\]

**Action regret** of a particular action is the difference between the optimal value and value of the action,
\\[\Delta_a = v_* - q(a)\\]

Therefore, the total regret becomes,
\\[L_t = \sum_{a \in \mathcal{A}} N(a)\Delta_a\\]

Where \\(N(a)\\) is the number of times action \\(a\\) is performed.

*Note that an agent doesn't know the true value of any action and therefore the optimal value. Hence, it can never know the total regret. It is just used for analysis of the algorithms, for example, the greedy policy has linear regret in the number of steps.*

#### \\(\epsilon\\)-greedy policy

One way to tradeoff between exploration and exploitation is to perform actions according to an \\(\epsilon\\)-greedy policy, i.e., perform the greedy (learnt) action with probability \\(1-\epsilon\\) and random action with probability \\(\epsilon\\).

But once you have explored enough, you won't really need a random action (since the learnt action values will tend to the true action values).Therefore, in practice, the value of \\(\epsilon\\) is reduced as the number of steps grow.