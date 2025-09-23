# Genie
*Written by Fatima Amrat*


## Problem Statement:
> [English](statements/genie_en.pdf)  
> [French](statements/genie_fr.pdf)  
> [Arabic](statements/genie_ar.pdf)  
> [Arabic (Algeria)](statements/genie_dz.pdf)  


## 1. Key Idea
Let `S` be the current sum of the drawn cards.  
Define `d = 21 - S`, which represents the maximum card value you can still draw without exceeding 21.  
- If the next card has value `≤ d`, it is safe → draw (`VUCI`).  
- If the next card has value `> d`, it is dangerous → stop (`DOSTA`). 
 

## 2. Proof
We want to show that the strategy can be reduced to a single condition.  
There are 10 possible card values: {2..9, 10, 11}, each appearing 4 times.  
- Safe cards = $4 \times (d - 1)$.  
- Dangerous cards = $4 × (11 - d)$.  

The condition `dangerous ≥ safe` simplifies to:  
$11 - d ≥ d - 1 \implies d ≤ 6 \implies S ≥ 15$

Therefore, the threshold occurs when S = 14:
- If S ≤ 13 → safe > dangerous → output `VUCI`.  
- If S ≥ 14 → dangerous ≥ safe → output `DOSTA`.  
 

## 3. Implementation

```cpp
#include <bits/stdc++.h>
using namespace std ;


int main() {
  int n, sum = 0;
  cin >> n;
  
  vector<int> numbers(n);
  for(int i = 0; i < n; i++) {
    cin >> numbers[i];
    sum += numbers[i];
  }

  if (sum < 14) cout << "VUCI\n";
  else cout << "DOSTA\n";

  return 0;
}
```

