# Kheira's Board Game

*Written by Iyed Baassou*

## Problem Statement

> [English](https://algerianoi.contest.codeforces.com/group/VICVhBwFUY/contest/554607/problem/B)

## Solution

We are asked to compute the total value of all cells in an $n \times n$ board starting at coordinates $(i, j)$. Each cell $(x, y)$ contributes $∣x−y∣+1$.

The simplest approach is to directly iterate over all cells, sum their contributions, and finally take the result modulo $10^9+7$.

This is $O(n^2)$ per test case which is enough for the constraint $n \le 1000$.

Note that we can take the modulo at every iteration, that would be vital for c++ solution in case of `long long` overflow.

## Implementation

```py
t = int(input())
    
for _ in range(t):
    i, j, n = map(int, input().split())
    res = 0
    
    for x in range(i, i + n):
        for y in range(j, j + n):
            res += abs(x - y) + 1

    res %= 10**9 + 7
    print(res)
```