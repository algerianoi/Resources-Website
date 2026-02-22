# Binary Exponentiation 
*Written by Raouf Ould Ali*

## Problem: Modular Exponentiation 💡

Consider the following problem:
- Write a program that outputs the remainder of the Euclidean division of $3^N$ by $M$, where $1 \leq N \leq 10^{18}$ and $1 \leq M \leq 10^9$.

## Straightforward Algorithm 📏

The straightforward algorithm would be the following:

```cpp
#include <bits/stdc++.h>
using namespace std;
int main(){
    long long N;
    int M;
    cin >> N >> M;

    long long result = 1;
    for(int i = 0; i < N; i++){
        result = result * 3;
    }
    
    cout << result % M << endl;
}
```

After trying $N = 10, M = 2000$, for example, we may think that we're done with the problem. But when trying with some higher values, we start getting some nonsense values. For instance, with $N = 100, M = 32000$, we get $-9263$, which doesn't make sense at all! This has a very simple explanation: as $3^N$ becomes larger and larger, it exceeds the maximum integer C++ can store in memory. Do you have any idea about how to solve this issue?

## Modular Arithmetic to the Rescue 🦸

Remember that we're not searching for the exact value, but only its remainder when divided by $M$. Hence, we can use the fact that:

$$ab \mod M = (a \mod M)(b \mod M) \mod M$$

This allows us to perform the computations by replacing at each step the variable `result` by its division remainder by $M$:

```cpp
#include <bits/stdc++.h>
using namespace std;
int main(){
    long long N;
    int M;
    cin >> N >> M;

    long long result = 1;
    for(int i = 0; i < N; i++){
        result = result * 3;
        result = result % M;
    }
    
    cout << result << endl;
}
```

Retrying $N = 100, M = 32000$ gives us the intended solution (2001). Let's goooo, we solved it! 🎉

## Optimizing for Large $N$ 🚀

Remember that the constraints say that $N$'s upper bound is $10^{18}$, but our algorithm obviously runs in $O(N)$, which is too slow (You can notice it yourself trying $N = 100000000000, M = 237564$). Can you figure out a solution?

Instead of multiplying $N$ times the `result` variable by $3$, you can notice the following facts:

$$3^{2k} = 3^{k} \times 3^{k}$$

$$3^{2k+1} = 3^{k} \times 3^{k} \times 3$$

This allows us to perform our calculation in $O(\log N)$ instead of $O(N)$, as to compute $3^{2k}$, we only need to compute $3^k$ once and just square it, as shown in the algorithm below:

```cpp
#include <bits/stdc++.h>
using namespace std;

long long ThreePow(long long k, int M){
    if(k == 0) return 1;
    long long subRes = ThreePow(k / 2, M); // If k is odd, k/2 computes its floor, aka (2j+1)/2 = j
    if(k % 2) // k is odd
        return (((subRes * subRes) % M) * 3) % M;
    else // k is even
        return (subRes * subRes) % M;
}


int main(){
    long long N;
    int M;
    cin >> N >> M;
    cout << ThreePow(N, M) << endl;
}
```

As we're dividing each time $k$ by $2$, it is obvious that the number of calls to our function is equal to $\lfloor \log_2 N \rfloor + 2$, giving a time complexity of $O(\log N)$. (You can try some test cases by hand on a piece of paper to figure it out.)

## Your Favorite Section: Problems 📚

- [UVA 374 - Big Mod](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=24&page=show_problem&problem=310)
- [LeetCode 1922 - Count Good Numbers](https://leetcode.com/problems/count-good-numbers/description/)
- [Codeforces 630I - Again Twenty Five!](https://codeforces.com/problemset/problem/630/I)
- [Math in Istanbul (made by me 😉)](https://codeforces.com/group/VICVhBwFUY/contest/553588/problem/A)

## If You Wanna Read More 📖

These articles give further explanations but also more advanced examples:

- [Binary Exponentiation - CP Algorithms](https://cp-algorithms.com/algebra/binary-exp.html)
- [Binary Exponentiation for Competitive Programming - GeeksforGeeks](https://www.geeksforgeeks.org/binary-exponentiation-for-competitive-programming/)

