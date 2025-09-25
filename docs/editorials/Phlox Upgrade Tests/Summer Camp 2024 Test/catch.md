# Catch Time

*Written by Iyed Baassou*

## Problem Statement

> [English](statements/task6-eng.pdf)  
> [Arabic](statements/task6-ara.pdf)  
> [French](statements/task6-fre.pdf)  

## Solution

Given the positions $a$ (Raouf), $b$ (Rassim) and $c$ (Redhouane).

Let's denote:  
`l = min(a, b)`: the leftmost catcher  
`r = max(a, b)`: the rightmost catcher

We can divide the problem into 3 different cases:

- If `c < l` then Redhouane is to the left of both Raouf and Rassim, which means his best option is moving as far as possible to the left (Keep moving left until he reaches cell 1 and waits there), this will allow him to survive `l - c` moves.

- If `c > r` then Redhouane is to the right of both Raouf and Rassim, the same previous scenario repeats in this case in which Redhouane will keep moving right until he reaches cell n and waits there, this will allow him to survive `c - r` moves.

- If `c < r` and `c > l` then Redhouane is trapped between Rassim and Raouf, his best move will always be moving towards the furthest catcher which will be tricky to track its move cost. But we know for sure Raouf and Rassim will keep shrinking Redhouane's space by 1 in each move and will catch him in `min(c - l, r - c)` moves.

## Implementation

For the sake of simplicity, Python will be used in implementation:

```py
n = int(input())
a = int(input())
b = int(input())
c = int(input())

l = min(a, b)
r = max(a, b)

if c < l:
    print(l - c)
elif c > r:
    print(c - r)
else:
    print(min(c - l, r - c))
```