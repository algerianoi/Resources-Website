# One Last Point

*Written by Iyed Baassou*

## Problem Statement

> [English](statements/task4-eng.pdf)  
> [Arabic](statements/task4-ara.pdf)  
> [French](statements/task4-fre.pdf)  

## Solution

Let's denote the array $a$ and integer $n$, size of $a$.

The most straightforward solution to this problem is trying every possible array that is a result of incrementing only one of the elements $a_i$.

This can be done by iterating through all elements in $a$, we increment the value of $a_i$, then we calculate the new product of the new $a$ and store the maximum product so far, without forgetting to reset the change by decrementing $a_i$.

Let's take a look at the second example:

n = 3  
a = [0, 1, 2]

For $i$ in range [0, n):  
- At i = 0, $a_0 = 0 + 1 = 1$ and the temporary array will be [1, 1, 2] and the product is 2
- At i = 1, $a_1 = 1 + 1 = 2$ and the temporary array will be [0, 2, 2] and the product is 0
- At i = 2, $a_2 = 2 + 1 = 3$ and the temporary array will be [0, 1, 3] and the product is 0

Thus, Hiba can achieve a maximum score of $2$ by getting one last point in the first problem.

## Implementation

For the sake of simplicity, we're going to use python for implementation:

- We'll use a function `prod(arr)` to easily find the product of an array while keeping clean structure (optional)

```py
def prod(arr):
    res = 1
    for i in range(0, len(arr)):
        res *= arr[i]
    return res


n = int(input())
a = []
for i in range(0, n):
    a.append(int(input()))

mx = 0
for i in range(0, n):
    a[i] += 1
    mx = max(mx, prod(a))
    a[i] -= 1

print(mx)
```