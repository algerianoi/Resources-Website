# Assil vs Ramses II

*Written by Iyed Baassou*

## Problem Statement

> [English](statements/task3-eng.pdf)  
> [Arabic](statements/task3-ara.pdf)  
> [French](statements/task3-fre.pdf)  

## Solution

\* We denoted the given number of boxes as $x$.

We can notice that the number of boxes in layer $i$ is $i²$

Thus we can find the number of layers by subtracting $1²$, $2^2$, $3^2$, ... , $k^2$ from $x$ until it's negative.

This can be reformulated as the greatest $i$ such that $\sum_{j = 1}^{i} j^2 \le x$

We can implement this with a while loop with the condition `sum + i**2 <= x` and we keep adding $i^2$ to `sum` and increasing the base $i$ of $i^2$ and the number of layers.

The output will be `layers` as number of layers and `x - sum` as the count of remaining boxes.

## Implementation

For sake of simplicity, we will be implementing the solution in Python.

```py
x = int(input())

i = 1
sum = 0

layers = 0

while sum + i**2 <= x:
    layers += 1
    i += 1
    sum += i**2

print(layers)
print(x - sum)
```