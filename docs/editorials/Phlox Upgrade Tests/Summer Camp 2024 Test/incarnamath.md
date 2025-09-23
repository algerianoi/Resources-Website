# IncarnaMath

*Written by Iyed Baassou*

## Problem Statement

> [English](statements/task2-eng.pdf)  
> [Arabic](statements/task2-ara.pdf)  
> [French](statements/task2-fre.pdf)  

## Solution

We simply print the product of `i` (the number of the student) and the latest number written on the chalkboard which is 27 at the very start.

To keep our approach correct, we update the latest number to that product.

This will repeat until the last student’s number is written.

We can do that using a for loop with variable `i` in the range `[1, n]` and we multiply `last` by `i`, given `last = 27` initially.

## Implementation

For sake of simplicity, we will be implementing the solution in Python.

```py
n = int(input())

last = 27
for i in range(1, n + 1): # Range [1, n + 1)
    print(last * i)
    last *= i

```