# Count the Ones 

*Written by Fatima Amrat*

- [Statement (English)] (docs/editorials/AOI College Cup/AOI College Cup 2024/statements/task-1.pdf)


## Key Idea:
- We are given a binary string S consisting of characters '0' and '1'. 
The task is to count the number of substrings (contiguous segments) that contain only the character '1'.

- Any valid substring must lie entirely within a block of consecutive '1's.
Therefore, instead of analyzing the whole string, it is enough to split it into maximal blocks of consecutive '1's and compute the contribution of each block separately.


## Formula:
- If the start is at position 1, there are k possible endings.
- If the start is at position 2, there are k-1 possible endings.
- If the start is at position 3, there are k-2 possible endings.
And so on, until we reach the start at position k, where there is exactly one ending.

So the total number of substrings in a block of length k is: 
$$
\frac{k \cdot (k+1)}{2}
$$


## Algorithm:
1. **Initialize variables**
   - `count = 0` → length of the current block of ones.
   - `ans = 0` → stores the total answer.

2. **Traverse the string from left to right**
   - If the character is `'1'`, increment `count`.
   - If the character is `'0'`, add `(count * (count + 1) / 2)` to `ans` and reset `count = 0`.

3. **After the loop**
   - Add the contribution of the last block of ones 
     (in case the string ends with `'1'`).

4. **Output**
   - Print the value of `ans`.
   
   
## Implementation (C++):

```cpp
#include <bits/stdc++.h>
using namespace std ;

int main(){
  string s ;
  cin >> s ;

  long long ans = 0 ;
  long long count = 0 ;

  for(int i = 0 ; i < s.size() ; i++){

    if(s[i]=='1'){
     count++;
    }

    else{
      ans += count * (count + 1) / 2LL;
      count = 0;
    }

  }

  ans += count * (count + 1) / 2LL ;
  cout << ans <<'\n' ;
  
}
```
