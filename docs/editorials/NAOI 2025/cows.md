
# Dox Taurus Cows

*By Sultan Alaiban*

## 1. Subtask 3 ($N,M ≤ 50, Q ≤ 20,000$)

We can simply generate all of the squares. For each square, we will maintain the number of cows in it. For each update, we can go through all the squares and check if the given cell is in it. For each query, we can go through all the squares and get the maximum. This works since there at most $N + M$ squares. This can be done in $\mathcal{O}((N + M) \cdot Q)$

## 2. Subtask 4 ($N$ is a multiple of $M$)

Since $N$ is a multiple of $M$, there will be $\frac{N}{M}$ squares. Cell $(i,j)$ will be contained in square $\lfloor \frac{j}{M} \rfloor$. Therefore, we can maintain the number of cells in each square with a `std::map` and maintain the maximum with a `std::multiset`.

Let $p = \lfloor \frac{j}{M} \rfloor$. To update, we will remove the `map[p]` from the multiset, increment/decrement `map[p]`, then reinsert `map[p]`.

To query, we can simply get the maximum value in the multiset, which we can do with `multiset.rbegin()`.

This can be done in $\mathcal{O}(Q \log Q)$.

## Full solution

To solve the problem, we will generalize the previous solution. If $N > M$, then there will be a group of $\lfloor \frac{N}{M} \rfloor$ squares of size $M$, and the rest of the grid will have $N' = N \bmod M$ rows and $M$ columns. The situation with $N ≤ M$ is similar.

For each group of squares, we can maintain the number of cows in each square like in Subtask 4. We will now show the number of groups we will generate in the end is $\mathcal{O}(log N + log M)$.

Assume $a > b$. Then, $a \bmod b ≤ a - b$. Moreover, $a \bmod b < b$ by definition. This means: $a \bmod b < \min(b, a - b + 1)$

By pigeonhole principle, $\min(b, a-b+1) ≤ \frac{a+1}{2}$, therefore: $a \bmod b < \frac{a+1}{2}$

Since $N$ can be halved at most $\log_2{N}$ times before reaching $0$, and $M$ can be halved at most $\log_2{M}$ times before reaching $0$, there at most $\log_2{N} + \log_2{M}$ groups of squares.

Updating and querying are similar to Subtask 4's solution, with an additional $\log N + \log M$ factor for locating the cell.

Total complexity is $\mathcal{O}(Q \log Q (\log N + \log M))$.

## Implementation

```cpp
#include <map>
#include <iostream>
#include <set>

using namespace std;
#define int long long
#define x first
#define y second

map<int, int> cnt;
multiset<int> cnts;

void find(int n, int m, int i, int j, int u, int index = 0) {
    if (n == 0 || m == 0) return;
    if (n > m) {
        int bottom_right_i = n - n%m;
        int bottom_right_j = m;
        if (0 <= i && i < bottom_right_i && 0 <= j && j < bottom_right_j) {
            index += i/m;
            cnts.extract(cnt[index]);
            cnts.insert(cnt[index] += u);
            return;
        }
        return find(n % m, m, i - (n/m)*m, j, u, index + n);
    } else {
        int bottom_right_i = n;
        int bottom_right_j = m - m%n;
        if (0 <= i && i < bottom_right_i && 0 <= j && j < bottom_right_j) {
            index += j/n;
            cnts.extract(cnt[index]);
            cnts.insert(cnt[index] += u);
            return;
        }
        return find(n, m % n, i, j - (m/n)*n, u, index + m);
    }
}

signed main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr), cout.tie(nullptr);

    int n,m,q; cin >> n >> m >> q;
    while (q--) {
        string s; cin >> s;
        if (s == "c") {
            cout << (cnts.empty() ? 0 : *cnts.rbegin()) << "\n";
        } else {
            int X,Y; cin >> X >> Y;
            int val = s == "a" ? +1 : -1;
            find(n, m, X, Y, val);
        }
    }
}
```
