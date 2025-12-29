# Problemset 03
*Prepared by Chams Eddine Abdelali Derreche*

## Problem 1

For a positive integer $m$ denote by $S(m)$ and $P(m)$ the sum and product, respectively, of the digits of $m$. Show that for each positive integer $n$, there exist positive integers $a_1, a_2, \ldots, a_n$ satisfying the following conditions: 

$S(a_1) < S(a_2) < \cdots < S(a_n) \text{ and } S(a_i) = P(a_{i+1}) \quad (i=1,2,\ldots,n).$

(We let $a_{n+1} = a_1$.)


## Problem 2

We call an even positive integer $n$ nice if the set $\{1, 2, \dots, n\}$ can be partitioned into $\frac{n}{2}$ two-element subsets, such that the sum of the elements in each subset is a power of $3$. For example, $6$ is nice, because the set $\{1, 2, 3, 4, 5, 6\}$ can be partitioned into subsets $\{1, 2\}$, $\{3, 6\}$, $\{4, 5\}$. Find the number of nice positive integers which are smaller than $3^{2022}$.

## Problem 3

A permutation of the integers $1, 2, \ldots, m$ is called fresh if there exists no positive integer $k < m$ such that the first $k$ numbers in the permutation are $1, 2, \ldots, k$ in some order. Let $f_m$ be the number of fresh permutations of the integers $1, 2, \ldots, m$.

Prove that $f_n \ge n \cdot f_{n - 1}$ for all $n \ge 3$.

For example, if $m = 4$, then the permutation $(3, 1, 4, 2)$ is fresh, whereas the permutation $(2, 3, 1, 4)$ is not.

## Problem 4

Let $n \ge k \ge 3$ be integers. Show that for every integer sequence $1 \le a_1 < a_2 < . . . <
a_k \le n$ one can choose non-negative integers $b_1, b_2, . . . , b_k$, satisfying the following conditions:
- $0 \le b_i \le n$ for each $1 \le i \le k$,
- all the positive $b_i$ are distinct,
- the sums $a_i + b_i$, $1 \le i \le k$, form a permutation of the first $k$ terms of a non-constant arithmetic progression.

## Problem 5

Find the smallest positive integer $k$ for which there exists a colouring of the positive integers $\mathbb{Z}_{>0}$ with $k$ colours and a function $f:\mathbb{Z}_{>0}\to \mathbb{Z}_{>0}$ with the following two properties:

$(i)$ For all positive integers $m,n$ of the same colour, $f(m+n)=f(m)+f(n).$

$(ii)$ There are positive integers $m,n$ such that $f(m+n)\ne f(m)+f(n).$

In a colouring of $\mathbb{Z}_{>0}$ with $k$ colours, every integer is coloured in exactly one of the $k$ colours. In both $(i)$ and $(ii)$ the positive integers $m,n$ are not necessarily distinct.