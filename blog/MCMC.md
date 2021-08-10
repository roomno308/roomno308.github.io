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

## MCMC = MC + MC

Intuitively, Markov chain is just a random walk on a graph (here, we only consider discrete time, finite state Markov chains). It has 2 components - the state space \\(\Omega\\), and the transition probabilities. The transition probabilities only depend on the current state, not on the past states i.e it is memory-less ([*Markov property*](https://en.wikipedia.org/wiki/Markov_property)). We can imagine \\(\Omega\\) as the vertices of a graph, and the transition probabilities as weights of the edges.

In our case, \\(\Omega\\) consists of all the \\(n!\\) permutations, and each edge has weight \\(\frac{1}{n}\\). The full graph for \\(n = 3\\) is shown below. At each node, we can pick the topmost card and place it at three different places (this includes the possibility of placing the card again at the top, and thus justifies the need of self loop).

![](https://i.imgur.com/UADXVav.png)
*<center>Graph for case n=3</center>*


Given a Markov chain, we can create a matrix known as the transition matrix \\(\mathbf{P}\\) (it looks something like [this](#1) for the above graph), where \\( {\mathbf{P}_{ij}} \\) represents the probability of moving from state \\(j\\) to state \\(i\\). Let the intital probability distribution over the states be \\(\mathbf{v}^0\\) i.e \\(\mathbf{v}_i^0\\) represents the initial probability of being at state  \\(i\\). We want to find the probability of being at state \\(i\\) after one time step.

\\[ 
\mathbf{v}^1_{i} = \sum_{j \in \Omega}(\text{Prob. of being in j at step 0)} \times (\text{Prob. of moving to i from j})
\\]
\\[
= \sum_{j \in \Omega}\mathbf{v}^0_{j}\mathbf{P}_{ij} 
\\]

where \\(\mathbf{v}^1\\) is probability distribution at the next step. This can be concisely written in matrix notation as 

\\[
\mathbf{v}^1 = \mathbf{P} \mathbf{v}^0
\\]

This is very nice, because we can now write the probability distribution at *any* later time \\(t\\):
\\[
\mathbf{v}^t = \mathbf{P}^t \mathbf{v}^0
\\]

### Steady State and Convergence
At steady state, the probabilities of being at any state do not change with time: 
\\[
\mathbf{v}^{t+1} = \mathbf{v}^t \implies \mathbf{P}\mathbf{v}^t = \mathbf{v}^t
\\]

The above equation is an *eigenvalue equation*, with eigenvalue 1. Thus, if the steady state exists, then it must necessarily be an eigenvector of the matrix \\(\mathbf{P}\\), with eigenvalue 1.  

We will not go into detail to show when a steady state exists for a general Markov chain, or when it is unique, or when and why the distribution converges to it, since that would be a bit of a long detour. We will however look at our specific example of the shuffling Markov chain, and show that the uniform distribution is a steady state, and that the Markov chain eventually converges to it.

For the shuffling Markov chain, the uniform distribution has \\(\mathbf{v}_i = \frac{1}{n!}\\) for all states \\(i\\). If the uniform distribution is the steady state, then from the eigenvalue equation, we have, from the eigenvalue equation,

\\[ \mathbf{v}_{i} = \sum_{j \in \Omega} \mathbf{P}_{ij} \mathbf{v}_{j} \\]

\\[
\iff \frac{1}{n!} = \sum_{j \in \Omega} \mathbf{P}_{ij} \frac{1}{n!} 
\\]


\\[
\iff \sum_{j \in \Omega} \mathbf{P}_{ij} = 1 
\\]

Thus, the uniform distribution is the steady state distribution if and only if \\(\sum_{j \in \Omega} \mathbf{P}_{ij} = 1\\) i.e the row sum of each of the rows is 1. Another way to think about it is that the sum of weights of edges going *into* each vertex is 1.

It is easy to verify that this is true for the shuffling chain. Each vertex has \\(n\\) edges going into it, with weight of each edge being \\(\frac{1}{n}\\)(for an example see the above graph for \\(n=3\\)).

### Mixing Time


We have shown that our algorithm converges to a uniform distribution; now, we want to  guarantee fast convergence. The *mixing time* is the time \\(t\\) when the probability distribution \\(\mathbf{v}^t\\) reaches "close" to the steady state distribution \\(\pi\\). Since our process is probabilistic, we want to get a bound on the *expected mixing time*.

To do this, we need to get some upper limit on the number of steps it takes to get to the uniform distribution, and then calculate the expected value of the number of steps. To do this, we first look at a seemingly distrinct problem.

#### (P) Bound on Mixing Time

Assume that, initially, the bottom-most card was \\(b\\), and after some \\(t\\) shuffles, it became the \\(3\\)rd card from the bottom. If the cards below \\(b\\) are \\(a_1\\) and \\(a_2\\), then the two permutations of \\(a_1\\) and \\(a_2\\) are equally likely (\\(\text{Pr}[(a_1,a_2)] = \text{Pr}[(a_2,a_1)] = 1 \cdot \frac{1}{2}\\) since the first card must be placed at the only available position below \\(b\\) and the next card can be placed in 1 of two positions below \\(b\\)). Similarly, if \\(b\\) is the \\(k\\)th card from the bottom after \\(t\\) shuffles, then each of the \\((k-1)!\\) permutations of the cards below \\(b\\) are equally likely. 

Now, assume that after \\(T\\) shuffles, the bottom-most card \\(b\\) became the topmost card. Then, all permutations of (\\(n-1\\)) cards below it are equally likely. So, in the next step, when \\(b\\) is put back into the deck, the probability ditribution over all permutations of the cards becomes uniform. Thus, we reach the steady state \\(\pi\\). Therefore, if we have an estimate of the expected value of \\(T\\), we have a bound on the mixing time.

#### (P) Estimating Mixing Time

Let \\(T_k\\) be the number of steps needed to move the card at \\(k\\)th position from bottom to the \\((k+1)\\)th position from bottom. Then, if \\(\mathbb{E}[X]\\) represents the expected value of the random variable \\(X\\), 

\\[
T = \sum_{k=1}^n T_k \implies \mathbb{E}[T] = \sum_{k=1}^n \mathbb{E}[T_k]
\\]

To estimate \\(T_k\\), we can find the probability distribution for \\(T_k\\):

\\[\text{Pr}[T_k = 1] = \frac{k}{n} \;\;\;\; (k \text{ positions below } b \text{ each with probability} \frac{1}{n})\\]


\\[\text{Pr}[T_k = 2] = \frac{n-k}{n} \cdot \frac{k}{n} \;\;\;\; ( \text{first card above } b, \text{ second card below } b)\\]

\\[\vdots\\]


\\[\text{Pr}[T_k = t] = \left(\frac{n-k}{n}\right)^{t-1} \cdot \frac{k}{n} \;\;\;\; ( \text{first } t-1 \text{ cards above } b,\; t^{th}\text{ card below } b)\\]

This is a commonly encountered probability distribution known as the geometric random variable, and the expected value for such a random variable turns out to be

\\[
\mathbb{E}[T_k] = \frac{n}{k}
\\]

Finally, we get an expression for expected value of \\(T\\):

\\[
\mathbb{E}[T] = n\sum_{k=1}^n \frac{1}{k} \approx n\log(n) 
\\]


### Appendix
-  **<a id="1">Transition matrix for n=3 case</a>**:
Let's label the topmost state "1", the next state going clockwise as "2" and so on...

<!-- <img src="https://render.githubusercontent.com/render/math?math=%5Cmathbf%7BP%7D%20%3D%20%5Cbegin%7Bbmatrix%7D%0A%5Cfrac%7B1%7D%7B3%7D%20%26%20%5Cfrac%7B1%7D%7B3%7D%20%26%200%20%26%200%20%26%20%5Cfrac%7B1%7D%7B3%7D%20%26%200%20%5C%5C%0A%5Cfrac%7B1%7D%7B3%7D%20%26%20%5Cfrac%7B1%7D%7B3%7D%20%26%200%20%26%200%20%26%200%20%26%20%5Cfrac%7B1%7D%7B3%7D%20%5C%5C%0A%5Cfrac%7B1%7D%7B3%7D%20%26%200%20%26%20%5Cfrac%7B1%7D%7B3%7D%20%26%200%20%26%200%20%26%20%5Cfrac%7B1%7D%7B3%7D%20%5C%5C%0A0%20%26%20%5Cfrac%7B1%7D%7B3%7D%20%26%200%20%26%20%5Cfrac%7B1%7D%7B3%7D%20%26%20%5Cfrac%7B1%7D%7B3%7D%20%26%200%20%5C%5C%0A0%20%26%200%20%26%20%5Cfrac%7B1%7D%7B3%7D%20%26%20%5Cfrac%7B1%7D%7B3%7D%20%26%20%5Cfrac%7B1%7D%7B3%7D%20%26%200%20%5C%5C%0A0%20%26%200%20%26%20%5Cfrac%7B1%7D%7B3%7D%20%26%20%5Cfrac%7B1%7D%7B3%7D%20%26%200%20%26%20%5Cfrac%7B1%7D%7B3%7D%0A%5Cend%7Bbmatrix%7D"> -->


\\[ \mathbf{P} = 
\begin{bmatrix}
\frac{1}{3} & \frac{1}{3} & 0 & 0 & \frac{1}{3} & 0 \\\\ 
\frac{1}{3} & \frac{1}{3} & 0 & 0 & 0 & \frac{1}{3} \\\\ 
\frac{1}{3} & 0 & \frac{1}{3} & 0 & 0 & \frac{1}{3} \\\\ 
0 & \frac{1}{3} & 0 & \frac{1}{3} & \frac{1}{3} & 0 \\\\ 
0 & 0 & \frac{1}{3} & \frac{1}{3} & \frac{1}{3} & 0 \\\\ 
0 & 0 & \frac{1}{3} & \frac{1}{3} & 0 & \frac{1}{3}
\end{bmatrix}\\]