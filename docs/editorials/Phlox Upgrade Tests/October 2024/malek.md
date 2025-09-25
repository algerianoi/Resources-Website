# Malek and Apples

*Written by Iyed Baassou*

## Problem Statement

> [English](https://algerianoi.contest.codeforces.com/group/VICVhBwFUY/contest/554607/problem/A)

## Solution

For each box $i$ Malek can sell $\lfloor \frac{a_i}{i} \rfloor$ maximum and the remainder will be $a_i\mod i$

So the final result is: $\sum_{i = 1}^n (a_{i - 1} \mod i)$ (0-based indexing)

## Implementation

```py
n = int(input())
a = list(map(int, input().split()))
    
rem = 0
    
for i in range(n):
    rem += a[i] % (i + 1)
    
print(rem)
```