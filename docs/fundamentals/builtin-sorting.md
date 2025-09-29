# Built-in Sorting in C++

## 1. Basic Usage

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    vector<int> v = {5, 2, 9, 1, 5, 6};

    // Sort in ascending order
    sort(v.begin(), v.end());

    for (int x : v) cout << x << " ";
}
```

**Output:**

```
1 2 5 5 6 9
```

* Works on any **random-access container** (`array`, `vector`, `deque`).
* Time complexity: **O(n log n)** on average.

---

## 2. Descending Order

```cpp
sort(v.begin(), v.end(), greater<int>());
```

---

## 3. Custom Comparators

A comparator is a function (or lambda) that returns **true** if the first argument should appear *before* the second.

### Example 1: Sort by absolute value

```cpp
sort(v.begin(), v.end(), [](int a, int b) {
    return abs(a) < abs(b);
});
```

### Example 2: Sort pairs by first, then second

```cpp
vector<pair<int,int>> vp = {{1,3},{1,2},{2,2},{2,1}};

sort(vp.begin(), vp.end(), [](auto &a, auto &b) {
    if (a.first != b.first) return a.first < b.first;
    return a.second < b.second;
});
```

**Output:**

```
(1,2) (1,3) (2,1) (2,2)
```

---

## 4. Rules for Comparators

* The comparators must impose a **strict weak ordering**:

  * No contradictions (`a < b` and `b < a` cannot both be true).
  * Transitivity : If `a < b` and `b < c`, then `a < c`.
* **Advice** : Always use `<` or `>` — avoid `<=` or `>=` inside comparators.

---

## 5. Keeping Original Indices When Sorting

Sometimes you want to sort values but **remember where they came from**.
The trick is to store **pairs `{index, value}`**.

```cpp
vector<int> a = {50, 20, 40, 10};
int n = a.size();

vector<pair<int,int>> vp; 
for (int i = 0; i < n; i++) {
    vp.push_back({i, a[i]}); // {index, value}
}

// Sort by value
sort(vp.begin(), vp.end(), [](auto &x, auto &y) {
    return x.second < y.second;
});

// Print sorted values with original indices
for (auto [i, val] : vp) {
    cout << "value = " << val << ", original index = " << i << "\n";
}
```

**Output:**

```
value = 10, original index = 3
value = 20, original index = 1
value = 40, original index = 2
value = 50, original index = 0
```

---

### Tip: Structured Bindings (C++17+)

You can unpack pairs directly in loops.

Instead of:

```cpp
for (auto p : vp) {
    int i = p.first;
    int val = p.second;
    cout << i << " " << val << "\n";
}
```

Write:

```cpp
for (auto [i, val] : vp) {
    cout << i << " " << val << "\n";
}
```

Cleaner, shorter, and easier to read.

---

## 6. Sorting Indices Instead of Values

Sort a list of **indices** according to the array values.

```cpp
vector<int> a = {50, 20, 40, 10};
int n = a.size();

vector<int> idx(n);
iota(idx.begin(), idx.end(), 0); // {0,1,2,3}

sort(idx.begin(), idx.end(), [&](int i, int j) {
    return a[i] < a[j];
});

// idx = {3,1,2,0}
```

---

## 7. Sorting with Explicit Tie-Breakers

When multiple keys matter, it is either needed or preferable to make the comparator **deterministic** (This means that no matter what the original order of the same set of elements is, the output order will always be the same).

```cpp
vector<pair<int,int>> vp = {{1,3},{1,2},{2,2},{2,1}};

sort(vp.begin(), vp.end(), [](auto &a, auto &b) {
    if (a.first != b.first) return a.first < b.first;
    return a.second > b.second; // second key descending
});
```

---

## 8. Stable Sort

`std::stable_sort` preserves the **relative order of equal elements**.
This is useful when you want to sort by multiple criteria in **stages** or maintain original ordering for equal keys.

### Example 1: Simple illustration of stability

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    vector<pair<int,int>> vp = {{1,200},{2,50},{1,100},{2,75}};

    // Sort by first element using stable_sort
    stable_sort(vp.begin(), vp.end(), [](auto &a, auto &b) {
        return a.first < b.first;
    });

    for (auto [x,y] : vp)
        cout << "(" << x << "," << y << ") ";
}
```

**Output:**

```
(1,200) (1,100) (2,50) (2,75)
```

✅ The **relative order of pairs with the same first element is preserved** (`200` before `100`, `50` before `75`).

---

### Example 2: Multi-stage sorting

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    vector<pair<int,int>> vp = {{1,3},{1,2},{2,2},{2,1}};

    // Step 1: Sort by second element (stable)
    stable_sort(vp.begin(), vp.end(), [](auto &a, auto &b) {
        return a.second < b.second;
    });

    // Step 2: Sort by first element (stable)
    stable_sort(vp.begin(), vp.end(), [](auto &a, auto &b) {
        return a.first < b.first;
    });

    for (auto [x,y] : vp)
        cout << "(" << x << "," << y << ") ";
}
```

**Output:**

```
(1,2) (1,3) (2,1) (2,2)
```

✅ The first-stage ordering (by second element) is preserved **within each group of equal first elements**.

---

**Notes:**

* Time complexity: **O(n log n)**
* Use `stable_sort` whenever you need **multi-level sorting** without writing a complex comparator.
