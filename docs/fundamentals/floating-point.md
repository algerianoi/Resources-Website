# Floating-Point Numbers

Floating-point numbers are used to represent real (decimal) numbers in computers. They are **approximations**, not exact values, which explains many surprising results in programs.

## What is a floating-point number?

A floating-point number is stored using **three parts**:

* **Sign (s)**: positive or negative
* **Exponent (e)**: scales the number
* **Mantissa / fraction (f)**: stores the significant digits

For normalized numbers, the value is:
$$
(-1)^s \times 1.f \times 2^{e-\text{bias}}
$$

This is similar to scientific notation, but in **base 2** instead of base 10.


## IEEE-754 formats (those used by C++)

Two formats matter most:


| Format              | Total Bits | Sign Bits | Exponent Bits | Fraction Bits | Effective Precision          | Exponent Bias | Approx. Decimal Precision |
| ------------------- | ---------- | --------- | ------------- | ------------- | ---------------------------- | ------------- | ------------------------- |
| binary32 (`float`)  | 32         | 1         | 8             | 23            | 24 bits (implicit leading 1) | 127           | ~7 digits                 |
| binary64 (`double`) | 64         | 1         | 11            | 52            | 53 bits (implicit leading 1) | 1023          | ~16 digits                |

Notes:

* *Effective precision* includes the implicit leading 1 used for normalized numbers.
* More fraction bits means more accurate representation of real numbers.
* In competitive programming, `double` is usually preferred due to its higher precision, but operations are a little be faster with `float`, when not much precision is needed.



## Why floating-point is tricky

### 1. Some decimals cannot be represented exactly

Example:

```text
0.1 + 0.2 != 0.3
```

This happens because `0.1` has an **infinite binary representation**, so it gets rounded.


### 2. Rounding errors

Each operation introduces a very small rounding error.
After many operations, these errors can **accumulate**.


### 3. Cancellation

Subtracting two nearly equal numbers loses precision.

Bad:
$$
\sqrt{1+x} - 1 \quad (x \text{ small})
$$

Better (reformulated):
$$
\frac{x}{\sqrt{1+x}+1}
$$


## Machine epsilon

**Machine epsilon** is the smallest number such that:
$$
1 + \varepsilon \neq 1
$$

For `double`:
$$
\varepsilon \approx 2.22 \times 10^{-16}
$$

This tells you the **limit of precision** you can expect.


## Comparing floating-point numbers (DO THIS)

❌ Bad:

```cpp
if (a == b)
```

✅ Good:

```cpp
if (abs(a - b) <= eps)
````


## Common competitive programming pitfalls

* Equality checks with doubles
* Summing many values in random order
* Using `float` instead of `double`
* Expecting exact decimal results

**Example:**

```text
1e9 + 1 - 1e9   // may give 0 instead of 1
```


## Best practices

- Avoid using floating-point numbers whenever possible. You can often rewrite the equations to keep everything in integers.
- If you must use them, prefer `double` over `float` for better precision.
- When comparing floating-point numbers, use an epsilon-based approach instead of direct equality.
- Be cautious of operations that can lead to significant precision loss, such as subtraction of nearly equal numbers.
- Consider using integer arithmetic or rational numbers if exact precision is required.

## Further Reading/Watching

- To understand better how floating-point numbers are stored in the memory, check out [this](https://www.puntoflotante.net/FLOATING-POINT-FORMAT-IEEE-754.htm).
- [Why Floats Will Drive You Crazy](https://www.youtube.com/watch?v=iE1grioxWS4)
- Pages 7 and 8 of Competitive Programming's Handbook by Antti Laaksonen.