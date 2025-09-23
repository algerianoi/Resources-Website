# Peas

*Written by Haithem Djefel*

## Problem Statement

> [English](statements/peas_en.pdf)  
> [Arabic](statements/peas_ar.pdf)  
> [Arabic (Algeria)](statements/peas_dz.pdf)  
> [French](statements/peas_fr.pdf)  

## 1. Idea

First, for convenience, we will index pods in the order in which their opening brackets appear (0 through n - 1), then, we can make a rooted tree with the given information, the root is pod 0, and for each two nodes $u$ and $v$, $v$ is a child of $u$ if and only if pod $v$ is contained inside pod $u$.

Now, letting $max_i$ denote the maximum value that can be obtained after squishing pod number $i$, we can achieve any value $s \leq max_i$ after squishing pod number $i$, this is true because all the values are real numbers.

For any leaf node $u$, $max_u = a_1$, and for any red pod $u$, $max_u = min(z_m, \sum_{c \in children_u} max_c)$, where $m$ is the number of children of $u$.

Now for the blue pods, we will use the following greedy approach, sort the children by their $max$ value in ascending order, and we keep processing them in this order one by one, for each child $v$, the maximum optimal value is $min(max_v,\frac{\text{max sum}}{\# \text{children left}})$, and $\text{max sum}$ starts with value $z_m$ ($m$ is the number of children), and keeps decreasing by the value we assign to each child.


## 2. Proof

*Note: if you want only the proof of the general case, you can skip this paragraph*

For the case of $max_u \geq \frac{z_m}{m}$, the proof directly follows from the Arithmetic mean-Geometric mean inequality, more formally, we need the maximum value for $\prod_{i = 1}^{n} a_i$ such that $\sum_{i = 1}^{n} a_i \leq z_m$

By the AM-GM inequality, we have the following:

$\sum_{i = 1}^{n} a_i \leq n\cdot\sqrt[n]{\prod_{i = 1}^{n}a_i}$

Dividing both sides by $n$ and raising to the power of $n$, we get the maximum value for the product, which holds iff $a_1 = a_2 = … = a_n = \frac{z_m}{m}$.

The latter case was not necessary to prove, as we will prove the more general case, but I liked to introduce it in the editorial, as it helps build the intuition and make it way more natural.

## 3. Intuition

From the previous idea we should notice that the numbers assigned to children must be “as close as possible”, now let’s formalize this.

## 4. General case

For the general case, we will prove it by contradiction, suppose there exists an optimal partition for the numbers with $a_i < min(\frac{z_m}{m}, max_i)$ for some $i$ (with the number of children > 1), then there exists some number $x \in \mathbb{R}$, that satisfies the following $a_i + x < min(\frac{z_m}{m}, max_i)$ and there exists an integer $j$ such that $a_j > a_i + x$, then $a_i * a_j > (a_i + x) * (a_j - x)$ must hold, expanding and reorganizing, this is equivalent to $a_i + x > a_j$, contradicting our assumption.

And we're done!


## 5. Implementation

Below is a code in C++ for this approach

```cpp
// Peas editorial by Haithem Djefel
#include <bits/stdc++.h>
using namespace std;

typedef double ll;
typedef pair<int, int> pii;

const int MOD = 1000000007;
const ll LOG = 31;

#define db(x) cerr << #x << " = " << x << " | "
#define dbg(x) cerr << #x << " = " << x << "\n"

#define Algerian ios::sync_with_stdio(0);
#define OI cin.tie(NULL);

vector<ll> mx;
vector<vector<ll>> ch;
vector<bool> type;
vector<ll> a;

void dfs(ll u) {
    // leaf node
    if (ch[u].size() == 0) {
        mx[u] = a[1];
        return;
    }
    
    // explore children first, if there are any
    for (ll v : ch[u]) dfs(v);
    
    sort(ch[u].begin(), ch[u].end(), [=](ll a, ll b) -> bool {
        return mx[a] < mx[b];
    });
    
    // if the pod is red
    if (!type[u]) {
        ll max_cap = a[ch[u].size()];
        ll s = 0;
        for (ll v : ch[u]) s += mx[v];
        
        mx[u] = min(s, max_cap);

        return;
    }

    // the pod is blue
    // left corresponds to the sum we're going to distribute, num to the number of unprocessed children
    ll left = a[ch[u].size()], num = ch[u].size(), val = 1;

    for (ll v : ch[u]) {
        // the number to assign to this child
        ll rm = min(mx[v], left / num);
        // adjust the number of children left and the total sum left to distribute
        --num;
        left -= rm;
        val *= rm;
    }

    mx[u] = val;
}

int main() {
    Algerian OI

    ll n;
    cin >> n;
    a.resize(n + 1);

    for (ll i = 1; i <= n; i++) cin >> a[i];

    string s;
    cin >> s;
    ll k = 0;

    for (auto c : s) if (c == '(') ++k; // getting the number of pods

    ch.resize(k);
    mx.resize(k);
    type.resize(k);

    stack<ll> st;
    ll idx = 0;

    // assigning each pod its index, children and type (blue or red)
    for (ll i = 0; i < (ll)s.size(); i++) {
        if (s[i] == '(') st.push(idx++);
        else if (s[i] == ')' && i != s.size() - 1LL) {
            ll cur = st.top();
            st.pop();
            ch[st.top()].push_back(cur);
        }
        else if (s[i] == '*') type[st.top()] = 1;
        else if (s[i] == '+') type[st.top()] = 0;
    }

    dfs(0);

    cout << fixed << setprecision(8);

    cout << mx[0] << "\n";

    return 0;
}
```
