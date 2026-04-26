# Dorm Labeling

## Problem Statement

- [English](statements/dormlabel(en).pdf)
- [French](statements/dormlabel(fr).pdf)
- [Arabic](statements/dormlabel(ar).pdf)

## Editorial

You are given a list of numbers $A[i]$, and you are asked to sort each number by 
$\{D[i], A[i], i\}$ where $\displaystyle{D[i] = \max_{j \neq i}(\gcd(A[i], A[j]))}$. 
<br> <br>
Constraints: $N \leq 10^6, A[i] \leq 10^6$

### Subtask 1: $N \leq 10^3$

This is your standard brute force subtask: for each number, just calculate their $D[i]$ by iterating over all of $A[i]$ and saving the maximum GCD witnessed so far. <br> <br>
For one pair of elements, there is a complexity of $O(\log(\max(A[i])))$ to compute the GCD. For each element, we need to compute $n - 1$ pairs, and there are $n$ elements so the complexity is $O(n^2\log(\max(A[i])))$. <br> <br>
The additional time alloted to sorting by {$D[i], A[i], i$} is $O(n\log n)$, which is dwarfed by the $O(n^2)$ term. The complexity doesn't change. A solution of this complexity solves the subtask for $N \leq 10^3$.

### Subtask 2: All $A[i]$ are prime.

The assumption that $A[i]$ are prime helps cut down the computation for each $D[i]$ immensely: the GCD of two primes numbers p, q is either $1$ (when $p \neq q$) or $p$ (when $p = q$). Consequently, each $D[i]$ is either $1$ or $A[i]$ for each $i$. Students can simply use a set data structure to detect duplicate primes ($O(\log n)$ per check) and set $D[i]$ accordingly. Since there are $n$ elements and $O(\log n)$ operations per element, the elements $D[i]$ can be determined in $O(n\log n)$ time. <br> <br>
Once again, sorting has the same complexity so the final solution complexity isn't any different. This solves the subtask for $N \leq 10^6$.


### Subtask 3: $A[i] \leq 10^3$.

This subtask was added to introduce the idea of eliminating duplicates. There are two important properties of GCD that are used here:

- For all integers $x$, we have $\gcd(x, x) = x$.
- The GCD between an element $x$ and any other integer is at most $x$.

These two properties are pretty intuitive: the largest number that can divide $x$ and $x$ is $x$, and there is no number larger than $x$ that divides 
$x$. <br> <br>
How are these two facts useful? Well, one can observe that if any $A[i]$ occurs more than once, we already know that $D[i]$ is atleast $gcd(A[i], A[i]) = A[i]$. However, we also know that there is no GCD larger than $A[i]$ obtainable between $A[i]$ and another number. We can thus eliminate all duplicate elements and set $D[i] = A[i]$ for said elements. There will be at most $10^3$ unique values remaining. We can use the quadratic time solution provided by the first subtask to compute remaining values $D[i]$. <br> <br>
This solves the problem in $O(n^2\log(\max(A[i])))$ time.

### Subtask 4: $A[i]$ are distinct integers between 1 and $N$.

For all numbers $A[i] \leq \lfloor \frac N 2 \rfloor$, we know that there exists a multiple $2 * A[i]$ in the array, and we know that $\gcd(A[i], 2*A[i]) = A[i]$, so their $D[i] = A[i]$. <br> <br> 
However, for numbers $A[i] > \lfloor \frac N 2 \rfloor$, we need a little more work: we need to find the largest divisor of each $A[i]$. One can observe that this is the same as finding the smallest divisor of $A[i]$, as we can just divide $A[i]$ by its smallest divisor to get its largest divisor. Since this divisor is always prime (there is a simple proof by contradiction), we simply have to use a modified eratosthenes' sieve to find the smallest prime that divides each $A[i]$.
This takes $O(n \log \log n)$ time and $O(n)$ space, which is doable for $N \leq 10^6$. <br> <br>
Once we have all $D[i]$, we simply sort, which gives the dominant $O(n\log n)$ term of our solution.

### Subtask 5: For all $D[i]$, we have atleast one $A[i]$ such that $A[i] = D[i]$.

The reasoning in the previous subtask in useful here as well, as a sieve style solution is needed. <br> <br>
The constraint has an interesting consequence: If each dorm has one $A[i]$ such that $D[i] = A[i]$, then all of the numbers with that dorm number are going to be multiples of the smallest value. Thus, finding the dorm number of any $D[i]$ consists of finding the largest $A[i]$ that divides it. Here is where the most important idea for this problem comes to mind: you may actually use each $A[i]$ to do this by setting $A[i]$ as the largest available multiple of $2*A[i],3*A[i], ...$ <br> <br>
The reason this is possible is that, in the worst case, you have to do at most $n + \frac n 2 + \frac n 3 + ... \approx n\log n$ reads and writes. Thus, you can compute the $D[i]$ of each element in $O(n\log n)$ steps. <br> <br>
After sorting, you have a final complexity of $O(n \log n)$, which solves the problem for our constraints.

### Subtask 6: Full Solution

The final solution for the problem consists of using the previous idea and doing the following for each $x$ between $1$ to $N$: if there exists two values $A[i], A[j]$ that are divisible by $x$, then we set their value as $D[i] = D[j] = x$. This works because the last number set in each $D[i]$ is the largest number that divides $A[i]$ and another number present in $A$. We can do this quickly over a frequency array in $O(n \log n)$ time (bound seen in previous subtask), and thus efficiently compute our $D[i]$ values. <br> <br>
This fully solves the task. 

Here is a sample implementation of the full solution:

```cpp
#include <bits/stdc++.h>
using namespace std;

int MAX = 1e6;

int main()
{
    
    ios_base::sync_with_stdio(0);
    cin.tie(NULL);

    int n, buf;
    cin >> n;
    
    vector<int> vs(n+1, 0), freq(MAX+1, 0);

    for(int i = 0; i < n; ++i){    
        cin >> vs[i];
        if(freq[vs[i]] != 0) freq[vs[i]] = vs[i];
        else freq[vs[i]] = 1;
    }
    

    for(int i = 2; i < MAX+1; ++i){
        
        int valmod = 0, prev;
        
        for(int j = i; j < MAX+1; j += i){

            if(freq[j]){

                if(valmod == 0) prev = j;
                else if(valmod == 1) freq[prev] = max(freq[prev], i);
                
                if(valmod > 0) freq[j] = max(freq[j], i);
                valmod++;

            }
            
        }
    }

    vector<int> sorting(n, 0);
    iota(sorting.begin(), sorting.end(), 0);
    
    sort(
        sorting.begin(), sorting.end(), 
        [&](int &a, int &b){
            
            if(freq[vs[a]] < freq[vs[b]]) return true;
            if(freq[vs[a]] > freq[vs[b]]) return false;

            if(vs[a] < vs[b]) return true;
            if(vs[a] > vs[b]) return false;

            return a < b;
        }
    );
    
    for(int i = 0; i < sorting.size(); ++i){
        cout << sorting[i] + 1; 
        if(i < sorting.size() - 1) cout << " ";
    }
    

    return 0;
}
```