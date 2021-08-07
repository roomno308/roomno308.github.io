---
layout: post
title: Markov Chain Monte Carlo
author: 308
---

<!-- 

Problem: shufle a deck of cards such that the final permutation is uniformly distributed across all permutations

Method:

Explanation:

Relation to MCMC (MC, MC)

MCMC (Properties, Mixing time etc.)

Deadline : 15th Aug 2021
 -->

<!-- Make this paragraph better -->
Suppose you are playing Blackjack with your friends, and you are asked to shuffle the deck of cards. You are socially awkward, and as such, do not want there to be allegations of misconduct on your behalf. You start overthinking and waste a lot of time thinking about ensuring a "fair" shuffle, and everyone thinks you are weird.

To avoid this potentially stressful situation, we present a procedure that you can follow. We also show how we can guarantee that it is a "ideal" way to shuffle, and in doing so, we shall introduce a powerful mathematical method for analyzing problems like this - the **Markov Chain Monte Carlo** (MCMC) method.

## The Problem

To formalize the problem, let us say that the deck consists of \\(n\\) cards numbered 1 to \\(n\\). Initially, the cards are in a known permutation. We want a simple set of moves such that, after repeatedly applying those moves a suitable number of times, the final permutation we get is "ideal".

So what does it mean for a shuffle to be "ideal"? Preferably, we would want our shuffle to have the following properties:

- **Uniform Convergence:** No one has any extra "knowledge" about the shuffle i.e. each of the \\(n!\\) permutations of the cards are equally likely. More formally, we want that the probability distribution of the shuffle is uniform over the set of all permutations. 
<!-- - We would want the final permutation that the shuffle results in to be uniformly randomly selected from all the (\\(n!\\)) possible permutations of the cards. -->
- **Fast:** We would want our shuffle to converge "fast". Note that "fast" here doesn't mean the speed at which your tiny fingers can shuffle the cards; instead, it means fast algorithmically, i.e. in the lowest possible number of moves.

## The Solution

There are a few well known methods of shuffling that satisfy the above properties. For the purpose of introducing MCMC, we choose one which is relatively simple, but is not optimal. The shuffling procedure is as follows:

1. Pick the top-most card and randomly place it anywhere in the deck (with equal probability).
2. Repeat step 1 "\\(k\\)" times (until your soul gives up).

Now, we consider the question of convergence and mixing - how can we be sure that the above procedure gives a good shuffle? How large should \\(k\\) be? This is where MCMC comes in.

### MCMC = MC + MC

Intuitively, a Markov chain is just a random walk on a graph (here, we only consider discrete time, finite state Markov chains). It has 2 components - the state space \\(\Omega\\), and the transition probabilities. The transition probabilities only depend on the current state, not on the past states i.e it is memory-less ([*Markov property*](https://en.wikipedia.org/wiki/Markov_property)). We can imagine \\(\Omega\\) as the vertices of a graph, and the transition probabilities as weights of the edges.

In our case, \\(\Omega\\) consists of all the \\(n!\\) permutations, and each edge has weight \\(\frac{1}{n}\\). The full graph for \\(n = 3\\) is shown below. At each node, we can pick the topmost card and place it at three different places (this includes the possibility of placing the card again at the top, and thus justifies the need of self loop).

![](https://i.imgur.com/UADXVav.png)
*<center>Graph for case n=3</center>*


Given a Markov chain, we can create a matrix known as the transition matrix \\(\mathbf{P}\\), where \\( {\mathbf{P}_{ij}} \\) represents the probability of moving from state \\(j\\) to state \\(i\\). Let the intital probability distribution over the states be \\(\mathbf{v}^0\\) i.e \\(\mathbf{v}_i^0\\) represents the initial probability of being at state  \\(i\\). We want to find the probability of being at state \\(i\\) after one time step.

\\[ 
\mathbf{v}^1_i = \sum_{j \in \Omega}(\text{Prob. of being in j at step 0)} \times (\text{Prob. moving to i from j})\\
\\]
\\[
= \sum_{j \in \Omega}\mathbf{v}^0_{j}\mathbf{P}_{ij} 
\\]

where \\(\mathbf{v}^1\\) is probability distribution at the next step. This can be concisely written in matrix notation as 

\\[
\mathbf{v}^1 = \mathbf{P} \mathbf{v}^0
\\]

Now, we want 
