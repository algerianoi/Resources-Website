# Rassim's Computer

*Written by Iyed Baassou*

## Problem Statement

> [English](https://algerianoi.contest.codeforces.com/group/VICVhBwFUY/contest/554607/problem/C)

## Solution

This is a straightforward problem since we only have to simulate Rassim’s computer step by step.

Good knowledge of [slicing](https://www.w3schools.com/python/python_strings_slicing.asp) and [dictionaries](https://www.w3schools.com/python/python_dictionaries.asp) would be very helpful in this problem.

We'll maintain a dictionary for the state of the registers and an index variable, and at each time we execute the corresponding operation and skip the number of characters it has in order to reach the next operations.

So:

- For type 1, `i += 8`.
- For type 2, `i += 3`.
- For type 3, `i += 3`.

## Implementation

```py
a = input()
memory = {'A': 0, 'B': 0, 'C': 0, 'D': 0}
    
i = 0
while i < len(a):
    if a[i] == '1':
        memory[a[i + 1]] += int(a[i + 2:i + 8])
        i += 8
    elif a[i] == '2':
        memory[a[i + 1]] += memory[a[i + 2]]
        i += 3
    elif a[i] == '3':
        memory[a[i + 1]] -= memory[a[i + 2]]
        i += 3
    
print(memory['A'], memory['B'], memory['C'], memory['D'])
```