# Binary Strings

*Written by Iyed Baassou*

- [Statement (English)](statements/task-7.pdf)


## Key observation

The number of binary strings of length $n$ is $2^n$

Let's try and prove it:

For $n=0$, there is exactly one binary string of length 0 (the empty string), and $2^0=1$, so our claim is true for this value of $n$.

Assume there are $2^k$ binary strings of length $k$. Any binary string of length $k+1$ can be formed by choosing a binary string of length $k$ (there are $2^k$ choices by the inductive hypothesis) and appending either `0` or `1` (2 choices). Thus there are $2^k⋅2=2^{k+1}$ binary strings of length $k+1$.

By induction the statement holds for all $n$.

## Subtask 1: $n \le 60$

$2^{60} \approx 1.15 \cdot 10^{18} \le 9.22 \cdot 10^{18}$

So $2^{60}$ fits in C++ `long long` type, which means we can easily calculate `(1LL << n) % MOD`.

## Subtask 2: $n \le 10⁷$

Well $2^{10⁷}$ is obviously **HUGE**.

So we need to instead multiply our result by 2 using `res = (res * 2) % MOD`. This runs in O(n) which is feasible for this subtask.

## Subtask 3: $n \le 10^{18}$

In order to solve this subtask and get the beautiful 100/100. We need to use [**Binary Exponentiation**](https://drive.google.com/file/d/1vTG_WTKnX6ZJhGy3JeJS9D1_69YxgYkV/view). This algorithm calculates $a^n$ in **logarithmic time**.

## Implementation (C++)

```cpp
#include <bits/stdc++.h>
using namespace std;

typedef long long ll;

const int MOD = 1e9 + 7;

ll binexp(ll a, ll b) {
    ll res = 1;
    while (b > 0) {
        if (b % 2) res = (res * a) % MOD;
        a = (a * a) % MOD;
        b >>= 1;
    }
    return res;
}

int main() {
    ll n; cin >> n;
    cout << binexp(2, n) << "\n";
}
```