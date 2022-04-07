---
layout: post
title: Hilbert Space
author: 308
---

Hilbert space is a special kind of vector space satisfying the following properties in addition to the properties of a vector space:

* Has an inner product operation:\\[<\psi_1, \psi_2> \in \mathbb{C} \\]
    * Conjugate symmetry: \\[<\psi_1, \psi_2> = <\psi_2, \psi_1>^*\\]
    * Linearity wrt. 2nd vector: \\[<\psi_1, a\psi_2 + b\psi_3> = a<\psi_1, \psi_2> + b<\psi_1, \psi_3>\\]
    * Antilinear wrt. 1st vector: \\[<a\psi_1 + b\psi_2, \psi_3> = a^{\*}<\psi_1, \psi_3> + b^{\*}<\psi_2, \psi_3>\\]
    * Positive definite: \\[<\psi, \psi> = \|\psi\|^2\\]
* Hilbert spaces are separable, i.e. they contain a countable, dense subset. (explanation after the properties)
* Hilbert Spaces are complete.

### Dense sets
A subset \\(X\\) of set \\(A\\) is called dense when every element in \\(A\\) is either in \\(X\\) or is a limit point of an element in \\(X\\).

For example, the set of rational numbers \\(\mathbb{Q}\\) is a dense subset of the set of real numbers \\(\mathbb{R}\\)

### Complete sets
A set \\(A\\) is said to be complete if all the cauchy sequences in \\(A\\) converge to some element in \\(A\\).
For example, \\(\mathbb{Q}\\) is not a complete set because the cauchy sequence \\(a_0 = 1, a_n = \frac{a_{n-1}}{2}+\frac{1}{a_n}\\) converges to \\(\sqrt{2}\\).