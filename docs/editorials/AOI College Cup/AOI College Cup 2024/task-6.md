# Kacem and Colored Balls

*Written by Haithem Djeffel*
- [Statement (English)](statements/task-6.pdf)

## Solution

Let $pref_i$ denote the sum $\sum_{j = 1}^{i} c_j$, and let $dp_i$ denote the number of ways to pull the first i colors of balls from the bag according to the rules given.

We can notice that to transition from pulling the first i colors to the first $i + 1$ colors, we need to pull a ball with the color $i + 1$ very last, so we don’t break our condition, and the remaining $c_{i + 1} - 1$ balls can be distributed in any way among the $pref_i$ balls, we can imagine it as a total of $pref_i + c_{i + 1} - 1 = pref_{i + 1} - 1$ balls, and we will choose $c_{i + 1} - 1$ of them to be of color $i + 1$, more formally, we have the recurrence: $dp_{i + 1} = dp_{i} \cdot \binom{pref_{i + 1} - 1}{c_{i + 1} - 1}, \forall i \in \{1, 2, ..,n - 1\}$.

We can precompute the factorial of every number from 1 to 1000 in linear time, and we can find the modular inverse of a factorial using Fermat’s little theorem with binary exponentiation, so there are n states, and it takes $O(\log m)$ to transition from a state, where m is the modulus, thus making the total time complexity $O(n \log m)$.

### Implementation

Below is a code in C++ for the described approach:

```cpp
#include <bits/stdc++.h>
using namespace std;

typedef long long ll;
typedef pair<int, int> pii;

const int MOD = 1000000007;
const ll LOG = 31;

#define db(x) cerr << #x << " = " << x << " | "
#define dbg(x) cerr << #x << " = " << x << "\n"

#define Algerian ios::sync_with_stdio(0);
#define OI cin.tie(NULL);

// array to precompute factorials
ll fact[1001];

// binary exponentiation
ll exp(ll b, ll e) {
    ll res = 1;

    while (e > 0) {
        if (e & 1) res = (res * b) % MOD;

        b = (b * b) % MOD;
        e >>= 1;
    }

    return res;
}

// a function that returns the modular inverse of k
ll inv(ll k) {return exp(k, MOD - 2);}

// n choose k
ll nck(ll n, ll k) {
    return (((fact[n] * inv(fact[n - k])) % MOD) * inv(fact[k])) % MOD;
}

int main() {
    Algerian OI

    ll n, a, res = 1, r = 0;
    cin >> n;

    fact[0] = 1;
    for (ll i = 1; i <= 1000; i++) {
        fact[i] = (fact[i - 1] * i) % MOD;
    }

    for (ll i = 0; i < n; i++) {
        // a is c_i, r is pref_i and res is dp_i
        cin >> a;
        r += a;

        res = (res * nck(r - 1, a - 1)) % MOD;
    }

    cout << res << "\n";

    return 0;
}
```
