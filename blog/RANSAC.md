---
layout: post
title: RANdom SAmple Consensus
author: 308
---

## The voting system

In the year 2999 in alternate reality, when everyone is fed up with the imperial governments, a new form of government is proposed. With the imperial government, most of the problems arise due to the incompetency of the ruler, who is selected by the previous king. The people who this new king would rule over have no say in the selection process. 

To remove this paradox, mathematicians propose a new form of government called **Consensus Government**. The main idea behind this consensus government would be that people would choose their own ruler.

## How will the consensus government work?

According to the proposal, the consensus government will have two types of rulers, *temporary rulers (TRs)* and a *king*. The temporary rulers will rule the country for a *grace period* of 4 months, and once a temporary ruler is promoted to the king, he will go on and rule the country for 10 years till the next elections.

Statisticians who have proposed the system believe that everyone among the population should have an equal chance to become a ruler, and therefore, the temporary rulers will be chosen uniformly at random from the people of the country. Once the selected person steps up as a TR, he will have 4 months to please the people. At the end of his 4-month term, there will be a final judgment day when people will either promote him to a king's position or dethrone him.

If he is dethroned, the process is repeated again until the country has a suitable ruler. On the contrary, if he is promoted, he rules the country for 10 years till the next election, when the whole process is repeated.

## Some obvious flaws

As you might have noticed already, there are some obious flaws in this system. Well, first of all, there can be big economic burden of conducting the voting of the judgement day every 4 months. Moreover, since random people are chosen to made the temporary ruler, the selected person might have to give-up his/her dayjob in order to become a ruler. Therefore, the system needs to account for the job security for the people who are elected.

Apart from these small flaws, there is a major flaw in the system. The main objective of a temporary ruler is to put 4-month effort to sway the judgement day voting to his favour. Now, this can be done in various ways one of which is pleasing people with your work. Another way of achieving this might be commiting a mass murder of the people who don't support you before the judgement day arrives.

Because of these flaws and potential risk of civil wars every 4-months, the system is discarded. But, the statisticians feel that there is more to it, they believe that there is a takeaway message. This system would work if humans are not allowed to commit a mass murder (or if the sample is not changing) ie. the population from which the person is selected to become a temporary ruler is the same as the population voting on the judgement day. These are the conditions that datapoints usually satisfy.

## RANSAC, Finally!!

Everything you have read till now might even be useless if you don't like to see things from an unconventional perspective but, I assure you that I won't deviate from the topic anymore.

RANdom SAmple Consensus, or RANSAC, is an iterative algorithm to estimate parameters of a mathematical model. For understanding the algorithm, we will take the task of fitting a line to given datapoints. We will also see that how RANSAC performs in the presence of outliers and do an analysis of its convergence.

![](https://lh3.googleusercontent.com/proxy/zHRkWxBbleexFl6gfUvPg0t-3NX4DgEeQPER3ALTLBHbeQihR4pOrUbsX4i7TkFrhs-vdoxylxN1QAO_mrEx9DZhH1G_zI0gGViAXj786sE8i3B_EFzDvCfFJZa62EoJgiqYj8waq-qd-i7Gz2-rq0kE)

<center>
<i>RANSAC in working. Source: [visual-experiments.com](http://www.visual-experiments.com/tag/ransac/)</i>
</center>