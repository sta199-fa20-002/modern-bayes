
---
title: "Module 8: The Multivariate Normal Distribution"
author: "Rebecca C. Steorts"
date: Hoff, Section 7.4
output: 
     beamer_presentation:
      includes: 
          in_header: custom2.tex
font-size: 8px
---

Agenda
===
- Motivational reading comprehension case study 
- Introduction/Review of vectors, matrices
- Population means/covariance matrices
- General multivariate notation
- Background on linear algebra (with practice exercises)
- Determinants, traces, quadratic forms

Agenda
===

- The multivariate normal distribution (MVN)
- Exercise with the MVN
- MVN-MVN semi-conjugacy 
- The inverse wishart distribution
- MVN-inverse wishart semi-conjugacy 
- The MVN-MVN-inverse wishart model 
- Applying a Gibbs sampler
- How to draw samples from the MVN and inverse wishart distributions
- Case study on reading comprehension

Goal
===

The goal of this module is to be able **to understand how to work with multivariate distributions**, such as the multivariate normal distribution. 

We also want to understand how univariate models that we have used in the past translate to the multivariate setting. 

Before we can delve in, we must review background on matrices, vectors, and **multivariate notation**. We also must review background on **linear algebra**. 

Example: Reading Comprehension
===


A sample of 22 children are given reading comprehension tests before and after receiving a particular instructional method.\footnote{This example follows Hoff (Section 7.4, p. 112).}

Each student $i$ will then have two scores, $Y_{i,1}$ and $Y_{i,2}$ denoting the pre- and post-instructional scores respectively. 

Denote each student’s pair of scores by the vector $\bm{Y}_i$
$$
\bm{Y}_{i} = \left( \begin{array}{c}
Y_{i,1}\\
Y_{i,2}\\
\end{array} \right) 
= \left( \begin{array}{c}
\text{score on first test}\\
\text{score on second test}\\
\end{array} \right)
$$
where $i=1,\ldots,n$ and $p=2.$

Example: Reading Comprehension
===

What does this data look like that is observed? 

$$\bm{X}_{n \times p} = 
\left( \begin{array}{cccc}
x_{11} & \textcolor{red}{x_{12}} & \ldots&  x_{n1}\\
x_{21} & \textcolor{red}{x_{22}} & \ldots& x_{n2} \\
x_{i1} & \textcolor{red}{x_{i2}} & \ldots& x_{ni} \\
\vdots & \vdots & \ddots & \vdots \\
x_{n1} & \textcolor{red}{x_{n2}} &\ldots& x_{np}
\end{array} \right).
$$

- A row of $\bm{X}_{n \times p}$ represents a covariate we might be interested in, such as age of a person.

- Denote $x_{i}$ $(p \times 1)$ as the $i$th \textcolor{red}{row vector} of the $X_{n \times p}$ matrix.

\[  x_{i}= \left( \begin{array}{c}
x_{i1}\\
\textcolor{red}{x_{i2}}\\
\vdots\\
x_{ip}
\end{array} \right) \]

Example: Reading Comprehension
===

We may be interested in the population mean $\bmu_{p \times 1}.$

$$
E[\bm{Y}] =: E[\bm{Y}_{i}] = \left( \begin{array}{c}
Y_{i,1}\\
Y_{i,2}\\
\end{array} \right) 
=  \left( \begin{array}{c}
\mu_1\\
\mu_2\\
\end{array} \right) 
= 
\bmu
$$

Example: Reading Comprehension
===
We also may be interested in the population covariance matrix, $\Sigma_{p \times p}.$

By definition: 
\begin{align}
\Sigma  
&= Cov(\bm{Y})\\
&=
\left( \begin{array}{cccc}
E[Y_1^2] - E[Y_1]^2 & E[Y_1Y_2] - E[Y_1]E[Y_2] \\
E[Y_1Y_2] - E[Y_1]E[Y_2] & E[Y_2^2] - E[Y_2]^2
\end{array} \right)\\
&=
\left( \begin{array}{cccc}
\sigma_1^2 & \sigma_{1,2} \\
\sigma_{1,2} & \sigma_2^2
\end{array} \right)
\end{align}

Remark: $Cov(Y_1) = Var(Y_1) = \sigma_1^2. \qquad Cov(Y_1, Y_2) = \sigma_{1,2}.$

How do we expand this beyond our reading comprehension example
===

We introduced our notation based upon a specific example to reading comprehension. 

How can we make this more general and applicable to general case studies and problems?

General Notation
===
Assume that $\bm{y}_{p \times 1} \sim (\mu_{p \times 1}, \Sigma_{p \times p}).$

$$\bm{y}_{p \times 1}= \left( \begin{array}{c}
y_{1}\\
{y_{2}}\\
\vdots\\
y_{p}
\end{array} \right).$$



$$\bmu_{p \times 1}= \left( \begin{array}{c}
\mu_1\\
\mu_2\\
\vdots\\
\mu_p
\end{array} \right)
$$
$$
\sig_{p \times p} = Cov(\bm{y}) =
\left( \begin{array}{cccc}
\sigma_1^2 & \sigma_{12} & \ldots&  \sigma_{1p}\\
\sigma_{21} & \sigma_2^2 & \ldots& \sigma_{2p}\\
\vdots & \vdots & \ddots & \vdots \\
\sigma_{p1} & \sigma_{p2} &\ldots& \sigma_p^2
\end{array} \right).
$$


Background
===

Before proceeding, we need to review some basic concepts from linear algebra:

1. Basic properties of matrices
2. Useful lemmas for working with matrices

The determinant of a matrix 
===

Assume a matrix $A_{n \times n}$  is invertible. The 
$$\det(A) = a_{i1}A_{i1} + a_{i2}A_{i2} + \cdots + a_{in}A_{in},$$
where $A{ij}$ are the co-factors and are computed from $$A_{ij} = (-1)^{i+j}det(M_{ij}).$$ 
$M_{ij}$ is known as the minor matrix and is the matrix you get if you eliminate row i and column j from matrix $A.$ You must apply this technique recursively.
\vskip 1em 

\textbf{We only use this technique when doing such calculations by hand or in proof-based approaches.}

The determinant of a matrix 
===

- How on earth do I use the complicated formula on the pervious slide. 

\textbf{Easy: Use the \texttt{det} command in \texttt{R} when faced with an application.}

\vskip 1em 

- You will also see a determinant in the definition of the multivariate normal distribution. 

\textbf{Important point: It's just a function and we typically do not need to evalute it in this course!}


The trace of a matrix 
===

Assume a matrix $H_{p \times p}.$

$$\text{trace}(H) = \sum_i h_{ii},$$ where $h_{ii}$ are the diagonal elements of $H.$

Interactive Exercise
===

We're going to move into working in groups, so you can work on the following exercises. 


The trace of a matrix 
===

$$
H =
\left( \begin{array}{cc}
1 & 0 \\
0 & 1 \\
\end{array} \right).
$$
What is $\text{tr}({H})?$ (Take 1 minute to complete this.)

Linear Algebra Tricks
===

Suppose that A is $n \times n$ matrix and suppose
that B is a $n \times n$ matrix. 

Lemma 1:  

$$tr(AB) = tr(BA)$$
Proof: Exercise. (Take 5-7 minutes to complete this.)



Linear Algebra Tricks
===

Lemma 2: 

Suppose $\bm{x}$ is a vector. $\bm{x}^TA\bm{x}$ is called a **quadratic form**. 

$$\bm{x}^TA\bm{x} = tr(\bm{x}^TA\bm{x}) = tr(\bm{x}\bm{x}^TA) = tr(A\bm{x}\bm{x}^T)$$

Proof: Exercise. 

Linear Algebra Tricks
===

Proof of Lemma 2:

\begin{align}
tr({\bm{x}^TA\bm{x}})  
&= \sum_i (x^tAx)_{ii} \\ 
& = (\bm{x}^T(A\bm{x})) \\
& = tr(A\bm{x}\bm{x}^T) \; (\text{by Lemma 1})
\end{align}

\begin{align}
tr({\bm{x}^TA\bm{x}})  
&= \sum_i (x^tAx)_{ii} \\ 
& = ((\bm{x}^TA)\bm{x}) \\
& = tr(\bm{x}\bm{x}^TA)\; (\text{by Lemma 1})
\end{align}




Notation
===
\begin{itemize}
\item MVN is generalization of univariate normal.
\item For the MVN, we write $\bm{y} \sim
\mathcal{MVN}(\bmu,\Sigma)$. 
\item The $(i,j)^{\text{th}}$
component of $\Sigma$ is the covariance between $Y_i$ and~$Y_j$ (so
the diagonal of $\Sigma$ gives the component variances).
\end{itemize}

Example: $Cov(Y_1, Y_2)$ is just one element of the matrix $\Sigma.$

Multivariate Normal
===
Just as the probability density of a scalar normal is
\begin{equation}
p(x) = {\left(2\pi\sigma^2\right)}^{-1/2}\exp{\left\{ -\frac{1}{2} \frac{(x-\mu)^2}{\sigma^2}\right\}},
\end{equation}
the probability density of the multivariate normal is
\begin{equation}
p(\bm{x}) = {\left(2\pi\right)}^{-p/2}(\det{\Sigma})^{-1/2} \exp{\left\{-\frac{1}{2} (\bm{x}-\bmu)^T\Sigma^{-1} (\bm{x} - \bmu)\right\}}.
\end{equation}
\textcolor{blue}{Univariate normal is special case of the multivariate normal with a one-dimensional mean ``vector'' and a one-by-one variance ``matrix.''}

Standard Multivariate Normal Distribution
===
Consider $$Z_1, \ldots, Z_n \stackrel{iid}{\sim} N(0,1).$$
Show that $$Z_1,\ldots,Z_n \sim MVN(0,I).$$

\begin{align}
f_z(z) &= \prod_{i=1}^n (2\pi)^{-1/2} e^{-z_i^2/2}\\
& = (2\pi)^{-n/2} e^{\sum_i-z_i^2/2}\\
& = (2\pi)^{-n/2} e^{-z^Tz/2}.
\end{align}

Exercise: Why does $\sum_i-z_i^2 = -z^Tz$?

We have just showed that $Z_1,\ldots,Z_n \sim MVN(0,I).$

Conjugate to MVN
===
Suppose that $$\bm{y} = (y_1 \ldots y_n)^T \mid \theta \sim MVN(\theta, \Sigma). $$
Let $$\pi(\btheta) \sim MVN(\bmu, \Omega). $$

What is the full conditional distribution of $\btheta \mid \bm{y}, \Sigma$?


Prior
===
\begin{align}
\pi(\btheta) &= {\left(2\pi\right)}^{-p/2}\det{\Omega}^{-1/2} \exp{\left\{-\frac{1}{2} (\btheta-\bmu)^T\Omega^{-1} (\btheta - \bmu)\right\}} \\
& \propto \exp{\left\{-\frac{1}{2} (\btheta-\bmu)^T\Omega^{-1} (\btheta - \bmu)\right\}} \\
& \propto \exp-\frac{1}{2} {\left \{\btheta^T\Omega^{-1} \btheta - 2 \btheta^T \Omega^{-1} \mu + \mu^T \Omega^{-1} \mu \right \}} \\
& \propto \exp-\frac{1}{2} {\left \{\btheta^T\Omega^{-1} \btheta - 2 \btheta^T \Omega^{-1} \mu  \right \}}\\
&= \exp-\frac{1}{2} {\left \{\btheta^TA_o \btheta - 2 \btheta^T  b_o  \right \}}
\end{align}
$\pi(\btheta) \sim MVN(\bmu, \Omega)$ implies that $A_o = \Omega^{-1}$ and $b_o = \Omega^{-1} \mu.$

Likelihood
===

\begin{align}
p(\bm{y} \mid \btheta, \Sigma) &= \prod_{i=1}^n
{\left(2\pi\right)}^{-p/2}\det{\Sigma}^{-1/2} \exp{\left\{-\frac{1}{2} (y_i-\btheta)^T\Sigma^{-1} (y_i - \btheta)\right\}}\\
\propto 
& \exp-\frac{1}{2} {\left \{ \sum_i y_i^T \Sigma^{-1} y_i -2 \sum_i \btheta^T \Sigma^{-1} y_i + 
\sum_i \btheta^T\Sigma^{-1} \btheta 
 \right \}}\\
 & \propto \exp-\frac{1}{2} {\left \{  -2 \btheta^T \Sigma^{-1} n\bar{y} + 
n \btheta^T\Sigma^{-1} \btheta 
 \right \}}\\
  & \propto \exp-\frac{1}{2} {\left \{  -2 \btheta^T b_1+ 
\btheta^T A_1 \btheta \right \}},
\end{align}
where $$b_1= \Sigma^{-1} n\bar{y}, \quad A_1 = n\Sigma^{-1}$$ and 
$$\bar{y} := (\frac{1}{n}\sum_i y_{i1} ,\ldots, \frac{1}{n} \sum_i y_{ip})^T.$$


Full conditional
===

\begin{align}
p(\btheta \mid \bm{y}, \Sigma) &\propto
p(\bm{y} \mid \btheta, \Sigma) \times p(\btheta) \\
&\propto 
\exp-\frac{1}{2} {\left \{  -2 \btheta^T b_1+ 
\btheta^T A_1 \btheta \right \}} \\
&\times
\exp-\frac{1}{2} {\left \{\btheta^TA_o \btheta - 2 \btheta^T b_o  \right \}}\\
%%%
&\propto \exp\{\btheta^T b_1 - \frac{1}{2}\btheta^T A_1 \btheta- \frac{1}{2}\btheta^TA_o  \btheta
+ \btheta^T b_o\}\\
& \propto\exp\{
\btheta^T( b_o + b_1) -\frac{1}{2}\theta^T(A_o + A_1) \theta
\}
\end{align}

Full conditional
===

From the previous slide, recall that 

$$p(\btheta \mid \bm{y}, \Sigma) \propto
\exp\{
\btheta^T( b_o + b_1) -\frac{1}{2}\theta^T(A_o + A_1) \theta
\}$$

Using the kernel of the multivariate normal, we can now find the posterior mean and the posterior covariance:

Then $$A_n = A_o + A_1 = \Omega^{-1}+n\Sigma^{-1}$$ and $$b_n = b_o + b_1 = \Omega^{-1}\mu + \Sigma^{-1} n\bar{y}$$
$$\btheta \mid \bm{y}, \Sigma \sim MVN(A_n^{-1}b_n, A_n^{-1}) = MVN(\mu_n, \Sigma_n).$$

Interpretations
===

$$\btheta \mid \bm{y}, \Sigma \sim MVN(A_n^{-1}b_n, A_n^{-1}) = MVN(\mu_n, \Sigma_n)$$
\begin{align}
\mu_n &= A_n^{-1}b_n = [\Omega^{-1}+n\Sigma^{-1}]^{-1}(b_o + b_1)\\
&=  [\Omega^{-1}+n\Sigma^{-1}]^{-1}(\Omega^{-1}\mu+ \Sigma^{-1} n\bar{y} )
\end{align}
\vskip 1em
\begin{align}
\Sigma_n &= A_n^{-1} = [\Omega^{-1}+n\Sigma^{-1}]^{-1}
\end{align}

inverse Wishart distribution 
===
Let us now consider a prior distribution on $\Sigma_{p \times p}$, which must be a positive definite matrix, meaning that $\bm{x}^T \Sigma \bm{x} > 0$ for all $\bm{x}.$

Suppose $\Sigma_{p \times p} \sim \text{inverseWishart}(\nu_o, S_o^{-1})$
where $\nu_o$ is a scalar and $S_o^{-1}$ is a matrix, where $\nu_o > p-1$ and $S_o$ must be positive definite.

It can be shown that 
$$E[\Sigma] = \frac{1}{\nu_0 - p -1}S_o.$$ (See Hoff, p. 110.)

Then $$p(\Sigma) \propto
\det(\Sigma)^{-(\nu_o + p +1)/2} \times \exp\{
-\text{tr}(S_o\Sigma^{-1})/2
\},$$
For the full distribution, see Hoff, Chapter 7 (p. 110).




inverse Wishart distribution
===
\begin{itemize}
\item The inverse Wishart distribution is the multivariate version of the Gamma distribution. 
\item The full hierarchy we're interested in is 
$$\bm{y} \mid \btheta, \Sigma \sim MVN(\btheta, \Sigma).$$ 
$$ \btheta \sim MVN(\mu, \Omega)$$
$$ \Sigma \sim \text{inverseWishart}(\nu_o, S_o^{-1}).$$
\end{itemize}
We first consider the conjugacy of the MVN and the inverse Wishart, i.e.
$$\bm{y} \mid \btheta, \Sigma \sim MVN(\btheta, \Sigma).$$ 
$$ \Sigma \sim \text{inverseWishart}(\nu_o, S_o^{-1}).$$

Continued
===
What about $p(\Sigma \mid \bm{y},  \btheta) \; \textcolor{red}{\propto} \; p(\Sigma) \times p(\bm{y} \mid \btheta, \Sigma).$
Let's first look at 
\begin{align}
&p(\bm{y} \mid \btheta, \Sigma) \\
&\propto
\det(\Sigma)^{-n/2}\exp\{-
\sum_i (\bm{y}_i - \btheta)^T\Sigma^{-1} (\bm{y}_i - \btheta)/2
\}\\
&\propto
\det(\Sigma)^{-n/2}\exp\{- tr(
\sum_i  (\bm{y}_i - \btheta)(\bm{y}_i - \btheta)^T\Sigma^{-1}/2)
\}\\
&\propto 
\det(\Sigma)^{-n/2}\exp\{-
\text{tr}(S_\theta \Sigma^{-1}/2)
\}
\end{align}
where $S_\theta = \sum_i (\bm{y}_i - \btheta) (\bm{y}_i - \btheta)^T.$

Note that $$\sum_k b_k^TA b_k = tr(B B^T A),$$ where B is the matrix whose $k$th row is $b_k.$ (Here we are applying Lemma 2.)

Continued
===
Now we can calculate $p(\Sigma \mid \bm{y},  \btheta)$
\begin{align}
&p(\Sigma \mid \bm{y},  \btheta) \\ & \quad= p(\Sigma) \times p(\bm{y} \mid \btheta, \Sigma) \\
& \quad \propto 
\det(\Sigma)^{-(\nu_o + p +1)/2} \times \exp\{
-\text{tr}(S_o\Sigma^{-1})/2
\} \\
& \qquad \times
\det(\Sigma)^{-n/2}\exp\{-
\text{tr}(S_\theta \Sigma^{-1})/2\}\\
& \quad \propto 
\det(\Sigma)^{-(\nu_o + n + p +1)/2}
\exp\{-
\text{tr}((S_o +S_\theta) \Sigma^{-1})/2\}
\end{align}
This implies that 
$$\Sigma \mid \bm{y}, \btheta \sim \text{inverseWishart}(\nu_o + n, [S_o + S_\theta]^{-1} =: S_n)$$

Continued
===
Suppose that we wish now to take 

$$\btheta \mid \bm{y}, \Sigma \sim MVN(\mu_n, \Sigma_n)$$ (which we finished an example on earlier).
Now let $$\Sigma \mid \bm{y}, \btheta \sim \text{inverseWishart}(\nu_n, S_n^{-1})$$
\vskip 1em 
There is no closed form expression for this posterior. Solution?

Gibbs sampler
===
Suppose the Gibbs sampler is at iteration $s.$
\begin{enumerate}
\item Sample $\btheta^{(s+1)}$ from it's full conditional:
\begin{enumerate}
\item[a)] Compute $\mu_n$ and $\Sigma_n$ from $\bm{y}$ and $\Sigma^{(s)}$
\item[b)] Sample $\btheta^{(s+1)}\sim MVN(\mu_n, \Sigma_n)$
\end{enumerate}
\item Sample $\Sigma^{(s+1)}$ from its full conditional:
\begin{enumerate}
\item[a)] Compute $S_n$ from $\bm{y}$ and $\theta^{(s+1)}$
\item[b)] Sample $\Sigma^{(s+1)} \sim \text{inverseWishart}(\nu_n, S_n^{-1})$
\end{enumerate}
\end{enumerate}

Working with Multivariate Normal Distribution
===
The `R` package, `mvtnorm`, contains functions for evaluating and simulating from a multivariate normal density.


```r
library(mvtnorm)
```

Simulating Data
===
Simulate a single multivariate normal random vector using the `rmvnorm` function.


```r
# Each row corresponds to a sample
# Here we have one sample (one row)
rmvnorm(n = 1, mean = rep(0, 2), sigma = diag(2))
```

```
##           [,1]      [,2]
## [1,] -0.850372 0.1580047
```


Evaluation
===
Evaluate the multivariate normal density at a single value using the `dmvnorm` function.


```r
dmvnorm(rep(0, 2), mean = rep(0, 2), sigma = diag(2))
```

```
## [1] 0.1591549
```

Working with the Multivariate Normal
===
- Now let's simulate many multivariate normals. 
- Each row is a different sample from this multivariate normal distribution.

```r
rmvnorm(n = 3, mean = rep(0, 2), sigma = diag(2))
```

```
##            [,1]       [,2]
## [1,] -0.2001285  0.9387704
## [2,] -1.1736533  0.1234839
## [3,] -0.4407347 -0.6498229
```


Work with the Wishart density
===
- The `R` package, `stats`, contains functions for evaluating and simulating from a Wishart density.

- We can simulate a single Wishart distributed matrix using the `rWishart` function.

- Each row is a different sample from the Wishart distribution. 


```r
nu0 <- 2
Sigma0 <- diag(2)
rWishart(1, df = nu0, Sigma = Sigma0)[, , 1]
```

```
##            [,1]       [,2]
## [1,] 1.11749075 0.06098128
## [2,] 0.06098128 0.09068899
```

An Application to Reading Comprehension
===
We will follow an example from Hoff (Section 7.4, p. 112). 

A sample of 22 children are given reading comprehension tests before and after receiving a particular instructional method. 

Each student $i$ will then have two scores, $Y_{i,1}$ and $Y_{i,2}$ denoting the pre- and post-instructional scores respectively, where $i=1,ldots n.$

Denote each student’s pair of scores $\bm{Y}_i$
$$
\bm{Y}_{i} = \left( \begin{array}{c}
Y_{i,1}\\
Y_{i,2}\\
\end{array} \right) 
= \left( \begin{array}{c}
\text{score on first test}\\
\text{score on second test}\\
\end{array} \right)
$$

Model set up
===
$$\bm{Y}_i \mid \btheta, \Sigma \sim MVN(\bm{\theta}, \Sigma).$$ 
$$ \bm{\theta} \sim MVN(\bm{\mu_0}, \Lambda_0)$$
$$ \Sigma \sim \text{inverseWishart}(\nu_o, S_o^{-1}).$$

Let $\btheta = (\theta_1, \theta_2).$

$i=1,\ldots,n.$

Prior settings
===
$$\bm{Y}_i \mid \bm{\theta}, \Sigma \sim MVN(\bm{\theta}, \Sigma).$$ 
$$ \bm{\theta} \sim MVN(\bm{\mu}_0, \Lambda_0)$$
$$ \Sigma \sim \text{inverseWishart}(\nu_o, S_o^{-1}).$$

The exam was designed to give average scores of around 50 out of 100, 
so $\bm{\mu}_0 = (50,50)^T$ would be a good choice for our prior mean.

Prior settings
===
$$\bm{Y}_i \mid \bm{\theta}, \Sigma \sim MVN(\bm{\theta}, \Sigma).$$ 
$$ \bm{\theta} \sim MVN(\bm{\mu}_0, \Lambda_0)$$
$$ \Sigma \sim \text{inverseWishart}(\nu_o, S_o^{-1}).$$

Since the true mean cannot be below 0 or above 100, we will use a prior variance that puts little probability outside of this range.

We’ll take the prior variances on $\theta_1$ and $\theta_2$ 
to be $$\lambda_{0,1}^2 = \lambda_{0,2}^2 = (50/2)^2 = 625$$ so that the prior probability that $P(\theta_j \neq [0,100]) =0.05.$

The two exams are measuring similar things, so we will take the prior correlation of 0.5 or rather $\lambda_{1,2} = 625/2 = 312.5$

Prior settings (continued)
===
$$\bm{Y}_i \mid \bm{\theta}, \Sigma \sim MVN(\btheta_j, \Sigma).$$ 
$$ \bm{\theta} \sim MVN(\bm{\mu}_0, \Lambda_0)$$
$$ \Sigma \sim \text{inverseWishart}(\nu_o, S_o^{-1}).$$

What about the prior settings for $\Sigma?$

We take $S_o$ to be about the same as $\Lambda_o.$

We will center $\Sigma$ around $S_o$ by setting 
$\nu_0 = p + 2 = 4,$ which is the mean of the inverse Wishart distribution (Hoff, p. 100).

Load in data
===

```r
# read in data
Y <- structure(c(59, 43, 34, 32, 42, 38, 55, 67, 64, 
                 45, 49, 72, 34, 70, 34, 50, 41, 52, 
                 60, 34, 28, 35, 77, 39, 46, 26, 38, 
                 43, 68, 86, 77, 60, 50, 59, 38, 48, 
                 55, 58, 54, 60, 75, 47, 48, 33), 
               .Dim = c(22L, 2L), .Dimnames = list(NULL, 
                c("pretest", "posttest")))
# number of observations
```

Quick calculations

```r
(n <- dim(Y)[1])
```

```
## [1] 22
```

```r
(ybar <- apply(Y,2,mean))
```

```
##  pretest posttest 
## 47.18182 53.86364
```

```r
(Sigma <- cov(Y))
```

```
##           pretest posttest
## pretest  182.1558 148.4069
## posttest 148.4069 243.6472
```

Application to reading comprehension
===

```r
# set hyper-parameters
mu0 <- c(50,50)
L0 <- matrix(c(625,312.5,312.5,625),nrow=2)
nu0 <- 4
S0 <- L0
```

Gibbs sampler
===

```
## Loading required package: coda
```

```
## Loading required package: MASS
```

```
## ##
## ## Markov Chain Monte Carlo Package (MCMCpack)
```

```
## ## Copyright (C) 2003-2020 Andrew D. Martin, Kevin M. Quinn, and Jong Hee Park
```

```
## ##
## ## Support provided by the U.S. National Science Foundation
```

```
## ## (Grants SES-0350646 and SES-0350613)
## ##
```

Gibbs sampler (review)
===
Suppose the Gibbs sampler is at iteration $s.$
\begin{enumerate}
\item Sample $\btheta^{(s+1)}$ from it's full conditional:
\begin{enumerate}
\item[a)] Compute $\mu_n$ and $\Sigma_n$ from $\bX$ and $\Sigma^{(s)}$
\item[b)] Sample $\btheta^{(s+1)}\sim MVN(\mu_n, \Sigma_n)$
\end{enumerate}
\item Sample $\Sigma^{(s+1)}$ from its full conditional:
\begin{enumerate}
\item[a)] Compute $S_n$ from $\bX$ and $\theta^{(s+1)}$
\item[b)] Sample $\Sigma^{(s+1)} \sim \text{inverseWishart}(\nu_n, S_n^{-1})$
\end{enumerate}
\end{enumerate}

Gibbs sampler
===

```r
THETA <- SIGMA <- NULL
set.seed(1)
for (s in 1:5000) {
  ## update theta
  Ln <- solve(solve(L0) + n*solve(Sigma))
  mun <- Ln %*% (solve(L0) %*% mu0 + 
                   n*solve(Sigma) %*% ybar)
  theta <- rmvnorm(1, mun, Ln)
  
  ## update Sigma
  Sn <- S0 + (t(Y) - c(theta)) %*% t(t(Y)-c(theta))
  
  Sigma <- solve(rwish(nu0 + n, solve(Sn)))
  ## save results
  THETA <- rbind(THETA, theta)
  SIGMA <- rbind(SIGMA, c(Sigma))
}
```

Posterior inference
===

Using the samples from the Gibbs sampler, we have generated 5,000 samples $$(\bm{\theta}^{(1)}, \Sigma^{(1)}, \ldots,\bm{\theta}^{(5000)}, \Sigma^{(5000)}) $$ that approxmiates $p(\bm{\theta}, \Sigma \mid y_1, \ldots, y_n).$

Glance at Gibbs sampler
===

```r
head(THETA)
```

```
##          [,1]     [,2]
## [1,] 45.76871 53.64765
## [2,] 43.84243 51.80471
## [3,] 43.41651 51.30521
## [4,] 46.85067 50.64238
## [5,] 42.62048 53.71350
## [6,] 50.32035 58.93397
```

```r
head(SIGMA)
```

```
##          [,1]     [,2]     [,3]     [,4]
## [1,] 270.7381 175.9276 175.9276 213.0155
## [2,] 237.3720 191.0999 191.0999 266.0570
## [3,] 245.6029 183.9140 183.9140 248.4452
## [4,] 169.6788 114.1658 114.1658 200.8390
## [5,] 247.0899 197.0802 197.0802 295.1981
## [6,] 289.4997 246.9184 246.9184 319.9786
```

Traceplot of $\theta_1$
===
![](08-multivariate-norm_files/figure-beamer/unnamed-chunk-12-1.pdf)<!-- --> 

Traceplot of $\theta_2$
===
![](08-multivariate-norm_files/figure-beamer/unnamed-chunk-13-1.pdf)<!-- --> 

Running average plot of $\theta_1$
===
![](08-multivariate-norm_files/figure-beamer/unnamed-chunk-14-1.pdf)<!-- --> 

Running average plot of $\theta_2$
===
![](08-multivariate-norm_files/figure-beamer/unnamed-chunk-15-1.pdf)<!-- --> 

Estimated density of $\theta_1$
===
![](08-multivariate-norm_files/figure-beamer/unnamed-chunk-16-1.pdf)<!-- --> 

Estimated density of $\theta_2$
===
![](08-multivariate-norm_files/figure-beamer/unnamed-chunk-17-1.pdf)<!-- --> 

Traceplots and running average plots
===

The traceplots don't tell us much of anything, so this is why we examine the running average plots. Specifically, the traceplots indicate that the chain has not failed to converged. 

The running average plots indicate that the sampler appears to be mixing well by 5,000 iterations and that the chain has not failed to converged. 



Traceplots and running average plots of $\sigma$
===
Examine the trace plots and running average plots of $\Sigma$ on your own.

Return to posterior inference
===
Given our samples from our Gibbs sampler, we can approximate posterior probabilities and confidence regions. 

Confidence regions
===

```r
quantile(THETA[,2] - THETA[,1], prob=c(0.025,0.5,0.975))
```

```
##      2.5%       50%     97.5% 
##  1.356260  6.614818 11.667128
```

Posterior inference
===
Suppose we were to give the exams/instruction to a large population, then would the average score on the second exam be higher than the first second? 

We can quanify this by calculating
$$Pr(\theta_2 > \theta_1 \mid y_1,\ldots y_n) = 0.99 
$$


```r
mean(THETA[,2] > THETA[,1])
```

```
## [1] 0.9926
```

Detailed Takeaways on Background 
===

- Understanding vectors, matrices and notation  
- Understanding how to write multivariate notation for a conceptual problem
- Understanding how to write general multivariate notation
- Background on linear algebra 
- Determinants, traces, quadratic forms
- Knowing how to do simple proofs such as the exercises from class 

Detailed Takeaways on Multivariate Normal Models
===

- The multivariate normal distribution (MVN)
- Exercise with the MVN
- MVN-MVN semi-conjugacy 
- The inverse wishart distribution
- MVN-inverse wishart semi-conjugacy 
- The MVN-MVN-inverse wishart model 
- Applying a Gibbs sampler
- How to draw samples from the MVN and inverse wishart distributions
- Case study on reading comprehension

Proof of Lemma 1
===

$$tr(AB) = tr(BA)$$

Proof: Suppose that $A_{n \times n}$ and $B_{n \times n}.$

Recall that by definition $tr(A) = \sum_{i=1}^n a_{ii}.$ By definition
\begin{align}
tr(AB) &= \sum_{i=1}^n (AB)_ii \\
& \sum_{i=1}^n \sum_{j=1}^n a_{ij} b_{ji}\\
& \sum_{i=1}^n \sum_{j=1}^n b_{ji} a_{ij} \\
&= \sum_{i=1}^n (BA)_ii 
= tr(BA)
\end{align}