---
title: CMU 15650 - Asymptotic Analysis
mathjax: true 
tags:
  - Algorithm
categories:
  - Technology
  - Algorithm
date: 2016-10-03 17:04:45
---
CMU 15650 - Algorithm and Data Structure 

Asymptotic Analysis
<!-- more -->

***

# Bounds
## Asymptotic Upper Bound
**Definitions $O()$:**
A runtime $T(n)$ is $O(f(n))$ if there exist constant $n_0 \geqslant 0$ and $c > 0$ such that:
$T(n) \leqslant cf(n)$ , for all $n \geqslant n_0 $

## Asymptotic Upper Bound
**Definitions $\Omega()$:**
A runtime $T(n)$ is $\Omega(f(n))$ if there exist constant $n_0 \geqslant 0$ and $\epsilon > 0$ such that:
$T(n) \geqslant \epsilon f(n)$ , for all $n \geqslant n_0 $

## Asymptotic Tight Bound
**Definitions $\Theta$():**
$T(n)$ is $\Theta(f(n))$ if $T(n)$ is $O(f(n))$ and $\Omega(f(n))$

**Theorem(Theta)**
If $lim_{n\rightarrow \infty} \frac{T(n)}{g(n)}$ equals some $c > 0$, then $T(n) = \Theta (g(n))$

Proof:
There is a $n_0$ such that $\frac{c}{2} \leqslant \frac{T(n)}{g(n)} \leqslant 2c$, for all $n > n_0$.

Therefore,for  $n > n_0$
 $T(n) \leqslant 2c g(n)$ , so $T(n) = O(g(n))$

Also, for  $n > n_0$
$ T(n) \geqslant \frac{c}{2} g(n)$ , so $T(n) = \Omega(g(n))$

# Properties of $O, \Omega, \Theta$
## Transity of $O$
If $f=O(g)$ and $g=O(h)$ then $f =O(h)$.

Solution:
By the assumptions,there are constants $c_f$, $n_f$, $c_g$, $n_g$ such that
$$f(n) ≤ c_f g(n)$$
and 
$$g(n) ≤ c_g h(n)$$ for all $n ≥ max\\{n_f ,n_g\\}$. 

Therefore
$$f(n) ≤ c_f g(n) ≤ c_f c_g h(n)$$
and setting $c = c_f c_g$ 
and $n_0 = max\\{n_f ,n_g\\}$.

Transity of $\Omega$ is similar

## Multiplication
For positive functions $f(n)$, $f ′(n)$, $g(n)$, $g′(n)$, if $f(n) = O(f′(n))$ and $g(n) = O(g′(n))$ then $$f(n)g(n) = O(f′(n)g′(n))$$

The proof is similar with Transitiviy

## Max
$max \lbrace f(n),g(n)\rbrace = Θ(f(n)+g(n))$ 
for any $f(n)$, $g(n)$ that eventually become and stay positive.

Proof:
1. Upper bound 
We have $max \lbrace f(n), g(n) \rbrace = O(f(n) + g(n))$ by taking $c = 1$ and
$n0$ equal to be the point where they become positive.

2. Lower bound
We have $max\lbrace f(n), g(n)\rbrace = \Omega (f(n) + g(n))$ taking the same $n0$ and $c = 1/2$ since the average of positive functions $f(n)$ and $g(n)$ is always less than the max.

## Polynomials
Prove that $an^2 + bn + d = O(n^2)$ for all real $a$, $b$, $d$.

Solution:
$an^2 + bn + d ≤ |a|n^2 + |b|n + |d|$,  for all $n ≥ 0$
$≤ |a|n^2 + |b|n^2 + |d|n^2$, for all $n ≥ 1$ 
$= (|a| + |b| + |d|)n^2.$ 
Take $c = |a| + |b| + |d|$ and $n0 = 1$.
 
## Polynomials and $\Theta$
Any degree-d polynomial $p(n) =  \sum_{i=0}^{d} αi n^i$ with $α_d > 0$ is $Θ(n^d)$.







