# Pi
Written by _Iyed Baassou_

## Problem Statement

- [English](statements/pi.pdf)


## Observations:
- The problem asks for the number of **non-decreasing** arrays of size $k$ that sums up to 
- The low constraint for $n$ ($1 \le n \le 250$) hints heavily to a 3-states DP.
### Part I: Recursion
This problem can be solved using recursion, since we, at each point, try different values that keep getting a sum of $n$ possible and keep the array **non-decreasing**.

This can be done by recursing through all indices $i$ ($0 <= i < k$) while tracking `sum so far` and `minimum possible value`.

Let's denote our recursive function as `pi(i, sum, mn)`.

Notice that we also need a way to make sure the array is still both **non-decreasing** and that we can continue from the current value to a **non-decreasing** array.

Notice that in the example $n = 8$ and $k=4$, is it not possible to use the value 3 at the first index. Because this forces us to use values greater or equal to 3 in the 3 remaining indices to get a sum of 8... Which is impossible because $3 \times 4 = 12 \ge 8$, while the value 2 allows us to use values greater or equal to 2 in the remaining 3 indices to get a sum of 8 because $2 \times 4 = 8 \le 8$

Thus we can conclude the following condition: $a_i \le \frac{(n - sum)}{(k - i - 1)}$.

This can be done to all indices and makes sure our final array is valid.

Let's now move to implementation, we will, for each value $j$ in $[mn, \frac{(n - sum)}{(k - i - 1)}]$, recurse to `pi(i + 1, sum + j, max(mn, j))`.

Once we reach $i = k - 1$ we increase the result if `sum` is equal to $n$.

#### Code:
```cpp
#include <bits/stdc++.h>
using namespace std;

int n, k;

int ways = 0;

void pi(int i, int sum, int mn) {
    if (i == k - 1) {
        if (sum == n) ways++;
        return;
    }

    for (int j = prev; j <= (n - sum) / (k - i - 1); ++j) {
        pi(i + 1, sum + j, max(mn, j));
    }
}

int main() {
    cin >> n >> k;

    for (int j = 1; j <= n / k; ++j) {
        pi(0, j, j);
    }

    cout << ways << "\n";

    return 0;
}
```
#### Time complexity:
In worstcase, the depth of the recursion is *k*.
Each depth tries approximately $n$ values.

Which causes a time complexity of $O(n^k)$ which is huge but the number of tries is very optimized in our approach which causes it to pass subtask 1 and subtask 2. (Considering weak testcases)

---
But this is not enough, we want a full solution, that beautiful green text or box. ✅

---
### Part II: DP Memoization

So the sole problem about recursion is recomputing values over and over again. Memoization is the best solution to this issue.

We'll simply only memoize the result for each recursion call `pi(i, sum, mn)` using a 3D DP table, and return it directly when recursion tries to recompute it.

#### Code:

```cpp
#include <bits/stdc++.h>
using namespace std;

int n, k;

int ways = 0;

int dp[250][250][250];

int pi(int i, int sum, int prev) {
    if (i == k - 1) {
        return dp[i][sum][prev] = (sum == n);
    }
    
    if (dp[i][sum][prev] != -1) return dp[i][sum][prev];

    int curr = 0;
    for (int j = prev; j <= (n - sum) / (k - i - 1); ++j) {
        curr += pi(i + 1, sum + j, max(prev, j));
    }

    return dp[i][sum][prev] = curr;
}

int main() {
    cin >> n >> k;

    memset(dp, -1, sizeof(dp));

    for (int j = 1; j <= n / k; ++j) {
        ways += pi(0, j, j);
    }

    cout << ways << "\n";

    return 0;
}
```

#### Time complexity:
Since the dynamic programming memoization handles the recomputations, the time complexity lowers down to both space and time complexities of $O(n^2\cdot k)$
