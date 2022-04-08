# EBM notes

### Rejection Sampling
Rejection sampling is a method to sample from complex distributions given that you know how to sample from some other distribution. 

Let's say that you want to sample uniformly from a complex set \\(\mathbf{A}\\) given that you know how to sample from the set  \\(\mathbf{B}\\), where \\(\mathbf{A} \subset \mathbf{B}\\). This method also assumes that you know a method to tell if an element belongs to \\(\mathbf{A}\\) or not.

![](https://i.imgur.com/FOGcQRV.png)

#### The algorithm (uniform case)
1. Sample \\(x \sim Uni(\mathbf{B}) \\)
2. **Accept** the sample if \\( x \in \mathbf{A} \\)
3.  Otherwise, **reject** and go to step 1. 

#### Lemma (uniform, discrete case)
If \\(\mathbf{A} \subset \mathbf{B}\\), \\(\{ X_1, X_2, \cdots, X_k\} \sim Uni(\mathbf{B}) \\), then \\(X_k \sim Uni(\mathbf{A})\\) where \\(k = \text{min}\{i : X_i \in \mathbf{A}\}\\)

##### Proof (uniform, discrete case):
Let \\(\mathbf{B} = \{Y_1, Y_2, \cdots, Y_n\} \\) and \\(\mathbf{A} = \{Z_1, Z_2, \cdots, Z_m\}\\).

Now, sampling \\(\{ X_1, X_2, \cdots, X_k\} \sim Uni(\mathbf{B}) \\) is a random process, we want to find the probability of \\(X_k\\) being one of the elements from \\(\mathbf{B}\\). Without the loss of generality, let's find the probability that \\(X_k = Z_1\\)

\\[
    P(X_k = Z_1) = \frac{1}{m} + \frac{n-m}{n} \cdot \frac{1}{m} + \left(\frac{n-m}{n} \right)^2  \cdot \frac{1}{m} + \cdots 
\\]

:: *First sample = \\(Z_1\\), second sample = \\(Z_1\\) and so on. Note that \\(\frac{n-m}{n}\\) denotes the probability of sampling anything from \\(\mathbf{B} - \mathbf{A}\\) because as soon as we sample something from \\(\mathbf{A}\\), the random experiment ends.*

\\[
    \implies P(X_k = Z_1) = \sum_{i = 0}^{\infty} \left(\frac{n-m}{n} \right)^i  \cdot \frac{1}{m}
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

**Lemma (i)**: If \\(X \sim q\\) and \\(Y|X \sim Uni(0, cq(X)) \\), then \\((X,Y) \sim Uni(\mathbf{B}) \\) where \\(\mathbf{B} = \{(x,y): x \in \mathbb{R}^d, 0 \le y \le cq(x)\} \\).

**Proof (i)**:
1. If \\(y \notin [0, cq(x)]\\), \\(p(x,y) = p(y|x)\cdotp(x) = 0\\)
2. Otherwise, \\(p(x,y) = p(y|x)\cdotp(x) = \frac{1}{cq(x)}\cdot q(x) = \frac{1}{c}\\)

Hence, uniform in \\(\mathbf{B}\\).

\
**Lemma (ii)**: If \\(\mathbf{A} \subset \mathbf{B}\\) and \\(\{ (x_1, y_1), (x_2, y_2), \cdots, (x_k, y_k) \} \sim Uni(\mathbf{B}) \\), then \\((x_k, y_k) \sim Uni(\mathbf{A})\\) where \\(k = \text{min}\{i: (x_i, y_i) \in \mathbf{A}\}\\).

**Proof**: 
Already covered in the [Proof](#proof-uniform-discrete-case) of Uniform, discrete case.

\
**Lemma (iii)**: If \\((X,Y) \sim Uni(\mathbf{A}) \\), where \\(\mathbf{A} = \{(x,y): x \in \mathbb{R}^d, 0 \le y \le f(x)\}\\), then \\(X \sim f \\).

**Proof**:
\\[
    p(X) = \int_{-\infty}^{\infty} p(X,Y) dy = \int_{-\infty}^{\infty} I(0, f(X)) dy = \int_{0}^{f(x)} dy = f(x)
\\]
Hence, \\(X \sim f\\)