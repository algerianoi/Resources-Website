# Job 
<em>Written by Mohamed Boukerche</em>

## Problem Statement
> [English](statements/job_en.pdf)  
> [Arabic](statements/job_ar.pdf)  
> [Arabic (Algeria)](statements/job_dz.pdf)  
> [French](statements/job_fr.pdf)  

## 1. Observations
- We are given 2 integers `N`  and `C`,  consider permutations of size `N` and  the **incorrectness** of a permutation is the number of inversions, more formally the count of pairs `(i, j)` with `i < j` and `A[i] > A[j]`.
- We should count how many permutations of length `N` have exactly `C` inversions with modulo $10^9+7$
## 2. Idea 

- Suppose we already have a permutation of size `i - 1`, and now we want to insert the element `i`.  
-  Depending on where we insert it, we add between `0` and `i - 1` new inversions:  
    - Insert at the end → add `0` inversions 
    - Insert before the last element → add `1` inversion 
    - Insert at the front → add `i - 1` inversions. 

Thus the number of inversions grows predictably.
    
This can be solved with Dynamic Programming (DP)

## 3. DP

### Definition
We define: $dp[i][j] =$ number of permutations of length `i` with exactly `j` inversions.


### Transition
$$
dp[i][j] = \sum_{k=0}^{\min(j, i-1)} dp[i-1][j-k]
$$

If we add the element `i` and place it such that it creates `k` new inversions, then the previous part must have `j - k` inversions.
 
### Base Case

$$dp[1][0] = 1$$


## 4. Optimization with Prefix Sums
- Naively, each transition costs $O(N)$, giving $O(C\cdot N²)$
- Using this naive approach may get partial points (~70%) because it is not efficient enough and optimizing the DP is necessary for full score.
- We can speed this up with **prefix sums**:

$$dp[i][j] = pre[j] - pre[j-i]$$

Where:

$$pre[j] = dp[i-1][0] + dp[i-1][1] + ... + dp[i-1][j]$$

This reduces the complexity to $O(N\cdot C)$.

## 5. Algorithm
1. Initialize `dp[0] = 1` (only one empty permutation).
2.  For each new number in 2..N:
   - Compute prefix sums of the current `dp`.
   - Use them to fill the new `dp`.
3. Print the answer `dp[C]` and don't forget modulo $10^9+7$.

## 6. Implementation (C++)
```cpp
/*
* Job problem by Mohamed boukerche 
*/
#include <bits/stdc++.h>
using namespace std;

const int MOD = 1e9 + 7;

#define ll long long
#define fastAOI ios::sync_with_stdio(false); cin.tie(nullptr);

int main() {
    fastAOI;
    ll n, c; cin >> n>> c;
    vector<ll> dp(c + 5); // For safety

    dp[0] = 1;
    
    vector<ll> a(c + 5);
    for (int i = 2; i <= n; i++) {
        vector<ll> pre(c + 5);
        pre[0] = dp[0];
        for(ll j = 1; j <= c; j++){
            pre[j] = pre[j - 1] + dp[j];
            pre[j] %= MOD;
        }
       for(int j = 0; j <= c; j++){
            ll l = pre[j], r = 0;
            if (j - i >= 0) r = pre[j - i];
            a[j] = (l - r + MOD) % MOD;
        }
        a.swap(dp); 
    }
    cout << dp[c];
    return 0;
}
```
## 7. Complexity
- Prefix sums allow each transition to be computed in $O(1)$.
- We have **`N` elements** and **`C` inversions**, so overall: $O(N\cdot C)$
