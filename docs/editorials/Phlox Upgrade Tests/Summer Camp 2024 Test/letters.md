# Magic Letters

*Written by Iyed Baassou*

## Problem Statement

> [English](statements/task5-eng.pdf)  
> [Arabic](statements/task5-ara.pdf)  
> [French](statements/task5-fre.pdf)  

## Solution

This is a frequency counting problem... with a twist.

We're simply going to use `collections:Counter()` which counts the number of occurances of each character in our array.

A cool trick we can use to make the job easier is reading the grid in uppercase since case doesn't matter, and that can be done using `.upper()` string method.

## Implementation

For the sake of simplicity, we're going to be using Python for implementation:

Note that the output needs to be sorted in alphabetical order.

```py
from collections import Counter

n = int(input())
s = ""
for i in range(0, n):
    s += input().strip().upper()

freq = Counter(s)

for c in sorted(freq):
    print(f"{c}: {freq[c]}")

```