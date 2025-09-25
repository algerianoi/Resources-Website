# Target Rotation
*Written by Muaath Alqarni*
## Statement
- [Arabic](https://naoi2025.algerianoi.com/statements/target%20(ar_DZ).pdf)
- [English](https://naoi2025.algerianoi.com/statements/target%20(en).pdf)
- [French](https://naoi2025.algerianoi.com/statements/target%20(fr).pdf)

## Solution
Observation 1: Each circular disc is independent of each other, so we need to precalculate for each disc.

Let $mn_{i,x,y}$ be the minimum cost to get value $x$ on the left and $y$ on the right.

Observation 2: If you are spinning the disc to get values $x$ and $y$, you will spin to get to either first or last $x$ from the left, after that you will continue spinning until you achive the first $y$ value on the right.

By observation 2, it is clear that you can only calculate for both ends of each number $x$, and it will be enough.

So here is the way: for every disc $i$ and number $x$. Calculate the first and last position of $x$. and for those positions, check the number on the right side of the disc. and the cost will be the minimum between distance clockwise and counterclockwise.

## Code
```c++
#include <bits/stdc++.h>
#define ll long long
using namespace std;

const int N = 1e5+1;
const ll INF = 2e17;

ll n, q;
ll a[10][N];
ll sz[N];

ll mn[N][10][10];

int get_ith_val(int idx, ll k) {
    k = (k-1)%sz[idx] + 1;
    for (int i = 0; i < 10; i++) {
        k -= a[i][idx];
        if (k <= 0) return i;
    }
    return -1;
}

void calc_for(int i, ll cur) {
    cur = ((cur-1)%sz[i])+1;
    int j = get_ith_val(i, cur);
    ll comp_pos = ((cur + sz[i]/2) - 1)%sz[i] + 1;
    int comp = get_ith_val(i, comp_pos);
    mn[i][j][comp] = min(mn[i][j][comp], min(cur-1, abs((sz[i]+1) - cur)));
    mn[i][comp][j] = min(mn[i][comp][j], min(comp_pos-1, abs((sz[i]+1) - comp_pos)));
}

int main()
{
    ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    cin >> n;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < 10; j++) {
            cin >> a[j][i];
            sz[i] += a[j][i];
        }
    }

    // precalculate
    for (int i = 0; i < n; i++) {
        for (int w = 0; w < 10; w++)
            for (int e = 0; e < 10; e++)
                mn[i][w][e] = INF;
        ll cur = 0;
        for (int j = 0; j < 10; j++) {
            calc_for(i, cur+1);
            cur += a[j][i];
            if (cur > 0)
                calc_for(i, cur);
        }
    }

    cin >> q;
    while (q--) {
        string s;
        cin >> s;
        ll sol = 0;
        for (int i = 0; i < n; i++) {
            sol += mn[n-i-1][s[2*n-i-1]-'0'][s[i]-'0'];
            if (sol > INF) break;
        }
        if (sol >= INF) cout << "IMPOSSIBLE\n-1\n";
        else cout << "POSSIBLE\n" << sol << '\n';
    }
}
```
