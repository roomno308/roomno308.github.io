---
layout: post
title: Energy Based Methods notes
author: 308
---

These notes are based on self-study on EBMs and don't follow any course curriculum strictly (I am just gathering ideas, reading papers). Basics of EBMs are covered in [this](EBMs.pdf) pdf. 

### GANs
TODO

### Rejection Sampling
Rejection sampling is a method to sample from complex distributions given that you know how to sample from some other distribution. 

Let's say that you want to sample uniformly from a complex set \\(\mathbf{A}\\) given that you know how to sample from the set  \\(\mathbf{B}\\), where \\(\mathbf{A} \subset \mathbf{B}\\). This method also assumes that you know a method to tell if an element belongs to \\(\mathbf{A}\\) or not.

![](https://i.imgur.com/FOGcQRV.png)

#### The algorithm (uniform case)
1. Sample \\(x \sim Uni(\mathbf{B}) \\)
2. **Accept** the sample if \\( x \in \mathbf{A} \\)
3.  Otherwise, **reject** and go to step 1. 

#### Lemma (uniform, discrete case)
If \\(\mathbf{A} \subset \mathbf{B}\\), \\(\\{ X_1, X_2, \cdots, X_k\\} \sim Uni(\mathbf{B}) \\), then \\(X_k \sim Uni(\mathbf{A})\\) where \\(k = \text{min}\\{i : X_i \in \mathbf{A}\\}\\)

#### Proof (uniform, discrete case):
Let \\(\mathbf{B} = \\{Y_1, Y_2, \cdots, Y_n\\} \\) and \\(\mathbf{A} = \\{Z_1, Z_2, \cdots, Z_m\\}\\).

Now, sampling \\(\\{ X_1, X_2, \cdots, X_k\\} \sim Uni(\mathbf{B}) \\) is a random process, we want to find the probability of \\(X_k\\) being one of the elements from \\(\mathbf{B}\\). Without the loss of generality, let's find the probability that \\(X_k = Z_1\\)

\\[
    P(X_k = Z_1) = \frac{1}{n} + \frac{n-m}{n} \cdot \frac{1}{n} + \left(\frac{n-m}{n} \right)^2  \cdot \frac{1}{n} + \cdots 
\\]

:: *First sample = \\(Z_1\\), second sample = \\(Z_1\\) and so on. Note that \\(\frac{n-m}{n}\\) denotes the probability of sampling anything from \\(\mathbf{B} - \mathbf{A}\\) because as soon as we sample something from \\(\mathbf{A}\\), the random experiment ends.*

\\[
    \implies P(X_k = Z_1) = \sum_{i = 0}^{\infty} \left(\frac{n-m}{n} \right)^i  \cdot \frac{1}{n}
\\]
\\[
    \implies P(X_k = Z_1) = \frac{1}{n}\cdot\frac{1}{\frac{m}{n}} = \frac{1}{m}
\\]

Since, \\(P(X_k = Z_1) = \frac{1}{\mathbf{card}(\mathbf{A})}\\), \\(X_k \sim Uni(\mathbf{A})\\)


#### Non-Uniform Case

Let's say that we need to sample from a complex distribution \\(f\\), given a "proposal distribution" \\(q\\) that we know how to sample from. Here, the support of \\(f\\) should be a subset of that of \\(q\\), i.e., \\( \exists c > 0 \ni cq(x) \ge f(x) \forall x \\).

#### Algorithm (Non-uniform case)
1. Pick a proposal distribution \\(q\\), such that \\(\exists c>0 \ni cq(x) \ge f(x) \forall x\\).
2. Sample \\(x \sim q\\)
3. sample \\(y \sim Uni(0, cq(x))\\)
4. **Accept** the sample \\(x\\) if \\(y \le f(x)\\)
5. Otherwise, **Reject** and go to step 2.

#### Lemma (Non-uniform case)
If we follow the [algorithm](#algorithm-non-uniform-case), we will end up sampling points from the distribution \\(f\\).

#### Proof

**Lemma (i)**: If \\(X \sim q\\) and \\(Y \mid X \sim Uni(0, cq(X)) \\), then \\((X,Y) \sim Uni(\mathbf{B}) \\) where \\(\mathbf{B} = \\{(x,y): x \in \mathbb{R}^d, 0 \le y \le cq(x)\\} \\).

**Proof (i)**:
1. If \\(y \notin [0, cq(x)]\\), \\(p(x,y) = p(y \mid x)\cdotp(x) = 0\\)
2. Otherwise, \\(p(x,y) = p(y \mid x)\cdotp(x) = \frac{1}{cq(x)}\cdot q(x) = \frac{1}{c}\\)

Hence, uniform in \\(\mathbf{B}\\).

\
**Lemma (ii)**: If \\(\mathbf{A} \subset \mathbf{B}\\) and \\(\\{ (x_1, y_1), (x_2, y_2), \cdots, (x_k, y_k) \\} \sim Uni(\mathbf{B}) \\), then \\((x_k, y_k) \sim Uni(\mathbf{A})\\) where \\(k = \text{min}\\{i: (x_i, y_i) \in \mathbf{A}\\}\\).

**Proof**: 
Already covered in the [Proof](#proof-uniform-discrete-case) of Uniform, discrete case.

\
**Lemma (iii)**: If \\((X,Y) \sim Uni(\mathbf{A}) \\), where \\(\mathbf{A} = \\{(x,y): x \in \mathbb{R}^d, 0 \le y \le f(x)\\}\\), then \\(X \sim f \\).

**Proof**:
\\[
    p(X) = \int_{-\infty}^{\infty} p(X,Y) dy = \int_{-\infty}^{\infty} I(0, f(X)) dy = \int_{0}^{f(x)} dy = f(x)
\\]
Hence, \\(X \sim f\\)

### MCMC
Markov Chain Monte Carlo is covered [here](https://roomno308.github.io/blog/MCMC.html)

## [Your GAN is Secretly an Energy-based Model and You Should Use Discriminator Driven Latent Sampling](https://arxiv.org/abs/2003.06060)

Now that we have covered all the prerequisites, let's dive right into this paper. This paper argues that in GAN, the generator and the discriminator collaborate to learn an "implicit" energy-based model. This energy function is only implicitly (and not directly) defined on the pixel space, and thus it is extremely difficult to sample directly from the pixel space. They propose a new Discriminator Driven Latent Sampling (DDLS) to produce better samples from the Generator.

*"Training of the GAN is an adversarial game which generally doesn't converge to the optimal generator, so usually \\(p_d\\) and \\(p_g\\) don't match perfectly"*. Here \\(p_d\\) is the true data distribution and \\(p_g\\) is the distribution generated by the generator. Even though \\(p_d\\) and \\(p_g\\) don't match, they claim that the discriminator has some idea about the mismatch of these distributions, and at its optimality (as presented in the vanilla GAN paper):

\\[
    D(x) \approx \frac{p_d(x)}{p_d(x) + p_g(x)}
\\]
Where \\(D(\cdot)\\) is the discriminator.

Consider \\(d\\) to be the logit of \\(D\\):
\\[
    d(x) = \log\left(\frac{D(x)}{1-D(x)}\right) \approx \log\left(\frac{p_d(x)}{p_g(x)}\right)
\\]

It can be verified that,
\\[
    p_d(x) \approx p_g(x)e^{d(x)}
\\]
Gives an EBM:
\\[
    p_d^* (x) = \frac{p_g(x)e^{d(x)}}{Z_0}
\\]
Where, \\(Z_0\\) is the normalization constant. Now, the paper claims that sampling \\(x\\) from \\(p_d^* \\) is better than sampling directly from \\(p_g\\) because:

1. It corrects generator's bias.
2. At discriminator's optimality, \\(p_d^* = p_d\\), even if the generator isn't optimal. (Can be easily verified at \\(D(x) = \frac{p_d(x)}{p_d(x) + p_g(x)}\\))

#### Proposition: 
**Given \\(p_g\\), we can sample from \\(p_d^* \\) using rejection sampling**
#### Explanation:
Given the proposal distribution \\(p_g\\), we sample \\(x \sim p_g\\) and accept the samples with the acceptance probability \\( \frac{p_d^* }{Mp_g} \\), where \\(Mp_g \ge p_d^* \\) or \\(M \ge \frac{p_d^* }{p_g}\\). This ensures that the accepted sample \\(y \sim p_d^* \\). (See [Rejection Sampling](#rejection-sampling) for proof)


But, sampling efficiently from \\(p_d^* \\) can be difficult because:
1. MCMC directly on pixel space \\(\mathcal{X}\\) will be inefficient, with long mixing time.
2. \\(p_g\\) is defined implicitly and can't be computed directly.

To overcome these problems, the paper proposes to sample directly from the latent space \\(\mathcal{Z}\\). We have 
\\[
\frac{e^{d(x)}}{Z_0} = \frac{p_d^* }{p_g}
\\]
\\[
    \implies \frac{e^{d(G(z))}}{Z_0} = \frac{p_d^* }{p_g}
\\]
Now, If we sample \\(z\\) from the latent space using prior distribution \\(p_0\\), and accept the sample with probabilty \\(\frac{e^{d(G(z))}}{MZ_0}\\), we are  doing the same thing as we proposed earlier. Therefore, the accepted sample \\(y \sim p_d^* \\).  (Note that \\(p_g = G \circ p_0\\), and thus the induced probability distribution on \\(\mathcal{Z}\\) becomes \\(p_t = e^{-E(x)}/Z'\\), where \\(E(x) = -\log p_0(x) -d(G(x))\\))