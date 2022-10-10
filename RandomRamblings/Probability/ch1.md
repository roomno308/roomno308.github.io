---
layout: post
title: Plausible reasoning
author: 308
---

## Deductive and Plausible reasoning

The book starts by presenting an imaginary situation to the reader: Suppose some dark night a policeman notices a masked man crawling out of a broken window of a jewelry store, carrying a bag full of expensive jewelry. Given this information, the policeman arrives at a conclusion that the man is a thief, but how does he arrive at this conclusion? After all, there are possible situations where a completely innocent man might find himself in such a position. How does reasoning work in presence of incomplete knowledge?

**Strong syllogism:**

Let's say for some statements A and B:

<center>
    If A is True, then B is True
</center>

<center>
    __
</center>
<center>
    A is True, therefore, B is True
</center>
<center>
    B is false, therefore, A is False
</center>
<center>
    __
</center>


"This is the kind of reasoning we would like to use all the time; but, as noted, in almost all the situations confronting us we do not have the right kind of information to allow this kind of reasoning"

**Weak syllogism:**
<center>
    If A is True, then B is True
</center>

<center>
    __
</center>
<center>
    B is True, therefore, A becomes "more plausible"
</center>
<center>
    __
</center>

Let's say you are guessing what pet your friend has. Somehow, you come to know that the pet is a bird, therefore, it being a parrot becomes more plausible (than without the information of it being a bird)

Therefore,

<center>
    A := The pet is a parrot, B:= The pet is a bird 
</center>

The presence of B makes A "more plausible".

**Weaker syllogism**
<center>
    If A is True, then B becomes more plausible
</center>

<center>
    __
</center>
<center>
    B is True, therefore, A becomes more plausible
</center>
<center>
    __
</center>

This is the kind of reasoning that the policeman follows in the situation described above. Here, A := The man is a thief, B := The man behaves fishy. we know that a thief is more likely to behave fishy in such situation (from our past knowledge?), the man is behaving fishy, therefore he is more likely to be a thief.

"In spite of the apparent weakness of this argument, when stated abstractly in terms of A and B, we recognize that the policeman’s conclusion has a very strong convincing power." "in our reasoning we depend very much on prior information to help us in evaluating the degree of plausibility in a new problem. This reasoning process goes on unconsciously, almost instantaneously, and we conceal how complicated it really is by calling it *common sense*."

With this understanding, the book takes on its goal as to build a robot which would carry out plausible reasoning, following clearly defined principles expressing an idealized common sense.

## Introduction to the robot

First, we set limits on the propositions our robot can reason about. The propositions must have unambiguous meaning, and should have a truth value (True or False). The truth value need not be ascertainable by any feasible investigation. Consider the following statements:

A := Beethoven and Berlioz never met

B := Beethoven is better than Berlioz

(For now) We only want to consider the statements of type A.

## Boolean Algebra

I won't go much deep into this section of the book as it just talks about basics of symbolic logic:

- The symbol \\( AB \\) denotes the proposition that both \\( A \\) and \\(B\\) are True.

- \\( A + B \\) denotes the proposition that at least one of \\(A\\) and \\(B\\) is True.

- \\(\bar{A}\\) denotes the proposition that \\(A\\) is False. Meanwhile \\(A\\) denotes the proposition that \\(\bar{A}\\) is False.

- \\( A \implies B \\) is the same as \\( \bar{A} + B \\), says that "if \\(A\\) then \\(B\\)".

| \\( A \\) | \\( B \\) | \\( AB \\) | \\( A+B \\) | \\( A \implies B \\) |
|-----------|-----------|------------|-------------|----------------------|
| 0         | 0         | 0          | 0           | 1                    |
| 0         | 1         | 0          | 1           | 1                    |
| 1         | 0         | 0          | 1           | 0                    |
| 1         | 1         | 1          | 1           | 1                    |

An interesting thing to notice here would be that the mathematical implication is not same as the implication used in English language. Unlike English, the mathematical implication doesn't denote causation: Every True statement implies every other True statement (Earth has one natural moon \\(\implies\\) these notes are written by Chaitanya). Moreover, every False statement implies every other statement regardless of the truth value (I am not writing this at 4:30 in the morning \\(\implies\\) God exists!).

One more important point to note here is the total number of prepositions that can be created from propositions \\(A\\) and \\(B\\) using the operators "or", "and" and "not". The prepositions are defined by the truth value they take for different combinations of values of \\(A\\) and \\(B\\) (TT, TF, FT, FF). For each of these pairs of values, our new proposition \\(C=f(A,B)\\) can be either True or False. Therefore there are a total of \\(2 \times 2 \times 2 \times 2\\) or \\(2^4\\) such prepositions. In general, given \\(n\\) propositions, \\(2^{2^n}\\) propositions can be constructed from them.

| \\(A\\) | \\( B \\) | \\(C\\) |
|---------|-----------|---------|
| T       | T         | T or F  |
| T       | F         | T or F  |
| F       | T         | T or F  |
| F       | F         | T or F  |

Moreover, these propositions can be written as composition of some standard propositions, thereby reducing the number of required propositions even further. (Book pg. 15 for further reading).

## Basic Desiderata

Now that we have sense of basic Boolean logic, we can now enumerate the goals we desire from our robot. If in future, the robot fails to adhere to these basic desiderata, we would know that there is something wrong with the way that we have developed our robot.

1. *Degrees of plausibility are represented by real numbers.*
    
    For our robot to work, the plausibility (for some state of knowledge in robot's "mind") must be represented by a physical quantity (eg. current/voltage).
    
2. *Degrees of plausibility must have a qualitative correspondence with common sense.*

    **Notation:** \\(A \mid C\\) denotes the plausibility of proposition \\(A\\) given that the proposition \\(C\\) is True. Now, \\(A \mid C > B \mid C\\) says that \\(A\\) is more plausible than \\(B\\), given \\(C\\). 

    "We do not want this robot to think in a way that is directly opposed to the way you and I think. So we shall design it to reason in a way that is at least qualitatively like the way humans try to reason, as described by the above weak syllogisms and a number of other similar ones."
    
    If some information \\(C\\) gets updated to \\(C'\\), such that,
    \\[
        A \mid C' > A \mid C
    \\]
    and,
    \\[
        (B \mid AC') = (B \mid AC)
    \\]
    This should only produce an increase, and never a decrease in the plausibility of both \\(A\\) and \\(B\\) being True. Formally,
    \\[
        (AB \mid C') \ge (AB \mid C)
    \\]

    Moreover,
    \\[
        (\bar{A} \mid C') < (\bar{A} \mid C)
    \\]
    
    Furthermore, "an infinitesimally greater plausibility ought to correspond only to an infinitesimally greater number." (continuity)

3. Consistency:
    a) Different ways of reasoning for a conclusion must result in same result.
    b) The robot always takes into account all of the evidence it has relevant to a question. It is not biased and nonideological.
    c) The robot always represents equivalent states of knowledge by equivalent plausibility assignments.
    
    a) and b) are quite easy to understand, while c) might require some more explanation. Let's say that we have provided a list of names as prior information to our robot, without providing any information about the person bearing that name / distinction between the names. Now we ask our robot a series of questions: 'How plausible is it that "Kobe" can dunk a basketball' or 'How plausible is it that "Chaitanya" can dunk a basketball'. As it turns out, Chaitanya can't. But the robot, since it doesn't know the distinction between the names "Kobe" and "Chaitanya", should assign equal plausibility as they represent equivalent (equivalently bad) states of knowledge.
    

This list of basic desiderata concludes the chapter 1 of the book (and our search for rules that the robot must obey). Chapter 2 takes on a challenge to derive a mathematical operator for plausibility consistent with these desiderata. 