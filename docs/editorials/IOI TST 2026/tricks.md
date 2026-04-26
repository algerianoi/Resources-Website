# Card Tricks

## Problem Statement

- [English](statements/tricks(en).pdf)
- [French](statements/tricks(fr).pdf)
- [Arabic](statements/tricks(ar).pdf)

## Solution

### Problem Analysis

We need to handle three operations on a collection of 32-bit integers:
1. **Insertion**: Add a number and its bit-reversed counterpart
2. **Retrieval**: Find and remove the largest number ≤ mean of current collection
3. **Flip**: Toggle between using original numbers or their bit-reversed versions

### Algorithm Design

#### Data Structures
1. **Two Multisets**: Maintain two collections - one for original values, one for reversed values
2. **Two Sums**: Track sums of both collections for efficient mean calculation
3. **State Flag**: Boolean to track whether we're using original or reversed values

#### Operations

1. **Insertion (Type 1)**:
   - Compute bit-reversed value of input
   - Based on current state, insert original into one multiset and reversed into the other
   - Update corresponding sums

2. **Retrieval (Type 2)**:
   - Calculate mean (sum/size) of current collection
   - Find largest value ≤ mean using upper_bound
   - Remove this value from both collections
   - Update sums accordingly

3. **Flip (Type 3)**:
   - Toggle the state flag

#### Bit Reversal
- For each bit position i (0-15):
  - Swap bit i with bit (31-i)
  - Preserve the original bit values during swap

### Complexity Analysis
- **Time**: O(log n) per operation (multiset operations)
- **Space**: O(n) to store all elements

### Key Implementation Notes
1. **Bit Reversal**: Must handle 32-bit unsigned integers correctly
2. **Mean Calculation**: Use integer division (floor division)
3. **Multiset Operations**: Ensure O(log n) time complexity for insertions, deletions, and searches
4. **State Management**: Cleanly toggle between original and reversed values

The algorithm efficiently handles all operations while maintaining the required constraints, making it suitable for large input sizes.

```cpp
#include <bits/stdc++.h>
using namespace std;

typedef long long ll;
typedef pair<ll, ll> pll;

const ll LOG = 32;

ll rev(ll x) {
    ll t = 0;

    for (ll i = 0; i < LOG; i++) {
        if ((x >> i) & 1) t |= (1LL << (LOG - i - 1));
    }

    return t;
}

int main() {

    ios::sync_with_stdio(0);
    cin.tie(NULL);

    ll q = 0;
    cin >> q;

    vector<ll> bits(LOG);
    vector<multiset<ll>> s(2);
    bool flip = 0;

    while (q--) {
        ll t;
        cin >> t;
        if (t == 1) {
            ll v;
            cin >> v;

            for (ll i = 0; i < LOG; i++) {
                if ((v >> i) & 1) bits[i]++;
            }
            s[flip].insert(v);
            s[!flip].insert(rev(v));
        } else if (t == 3) {
            flip = !flip;
            auto c = bits;
            for (ll i = 0; i < LOG; i++) {
                bits[i] = c[LOG - i - 1];
            }
        } else if (t == 2) {
            ll mean = 0;
            for (ll i = 0; i < LOG; i++) {
                mean += (1LL << i) * bits[i];
            }
            mean /= s[flip].size();
            ll res = *(--s[flip].upper_bound(mean));
            for (ll i = 0; i < LOG; i++) {
                if ((res >> i) & 1) bits[i]--;
            }
            s[flip].erase(s[flip].find(res));
            s[!flip].erase(s[!flip].find(rev(res)));
            cout << res << "\n";
        }
    }

    return 0;
}
```