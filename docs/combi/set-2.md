# Problemset 02

*Prepared by Chams Eddine Abdelali Derreche*

## Problem 1

For each positive integer $n$, find the number of $n$-digit positive integers that satisfy both of the following conditions:
- no two consecutive digits are equal, and
- the last digit is a prime.

## Problem 2

Determine the smallest positive integer $k$ with the following property. For any subset $\mathbb S$ of the set ${1,2,3,..., 2024}$ with $|\mathbb S| = k$, there are two distinct elements $a,b \in \mathbb S$ such that $ab+1$ is a perfect square.

## Problem 3

We call a $5-$tuple of integers *arrangeable* if its elements can be labeled $a,b,c,d,e$ in some order so that $a-b+c-d+e = 29$. Determine all $2017-$tuples of integers $n_1, n_2,...,n_{2017}$ such that if we place them in a circle in clockwise order, then any $5-$tuple of numbers in consecutive positions on the circle is arrangeable.

## Problem 4

Let $n$ be a positive integer such that $n^2-1 \over 6$ is divisible by 6. An $n \times n$ board is given. Prove that it is possible to place $n^2-1\over 6$ non-overlapping right triangles on the board with the lengths of $3,4,5$.

## Problem 5

In each square of a garden shaped like a $2022\times2022$ board, there is initially a tree of height 0 (A real tree dear programmers !). A gardener and a lumberjack alternate turns playing the following game, with the gardener taking the first turn:

- The gardener chooses a square in the garden. Each tree on that square and all the surrounding squares (of which there are at most eight) then becomes one unit taller.
- The lumberjack then chooses four different squares on the board. Each tree of positive height on those squares then becomes one unit shorter.

We say that a tree is _majestic_ if its height is at least $10^6$. Determine the largest $K$ such that the gardener can ensure that there are eventually $K$ majestic trees on the board, no matter how the lumberjack plays.