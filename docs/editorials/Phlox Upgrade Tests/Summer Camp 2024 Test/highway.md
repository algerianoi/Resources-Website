# East-West Highway

*Written by Iyed Baassou*

## Problem Statement

> [English](statements/task1-eng.pdf)  
> [Arabic](statements/task1-ara.pdf)  
> [French](statements/task1-fre.pdf)  

## Solution

By definition, the distance from $0$ to $x$ is denoted as $|x - 0|$, and it's called the absolute value of $x$. This can be applied to the distance from $a$ to $b$ as $|a - b|$.

---

### Solution 1:

Fortunately, most programming languages like C++ and python provide a built-in function to calculate absolute value called `abs()` which makes the task way easier.

*Note: the following solutions are just for the fun of the reader and are not a good practice when you have an easy solution like Solution 1*

---
### Solution 2:


We can also solve this without `abs()` using the definition of absolute value:

$$
|x| =
\begin{cases}
x, & \text{if } x \geq 0 \\
-x, & \text{if } x < 0
\end{cases}
$$

Applying it to $a - b$:

$$
|a - b| =
\begin{cases}
a - b, & \text{if } x \geq 0 \\
b - a, & \text{if } x < 0
\end{cases}
$$

---
### Solution 3:

Another cool way to solve it is observing that $|x| = max(x, -x)$

Which translates to $|x| = max(a - b, b - a)$

## Implementation

For sake of simplicity, we will be implementing solutions in Python.

```py
# Solution 1:
a = int(input())
b = int(input())
print(abs(a - b))
```

```py
# Solution 2:
a = int(input())
b = int(input())
if a - b >= 0: print(a - b)
else print(b - a)
```

```py
# Solution 3:
a = int(input())
b = int(input())
print(max(a - b, b - a))
```
