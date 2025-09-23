# Editorial -- Task 1: Count the Ones

## Problem Restatement

We are given a binary string S consisting of characters '0' and '1'. The
task is to count the number of substrings (contiguous segments) that
contain only the character '1'.

## Key Observation

Any valid substring must lie entirely within a block of consecutive
'1's. Therefore, instead of analyzing the whole string, it is enough to
split it into maximal blocks of consecutive '1's and compute the
contribution of each block separately.

## Derivation of the Formula

Suppose we have a block of consecutive ones of length k.

-   If the start is at position 1, there are k possible endings.
-   If the start is at position 2, there are k-1 possible endings.
-   If the start is at position 3, there are k-2 possible endings.
-   ...
-   If the start is at position k, there is exactly one ending.

So the total number of substrings in a block of length k is:

    k + (k-1) + (k-2) + ... + 1 = k * (k+1) / 2

## Algorithm

1.  Initialize `count = 0` and `ans = 0`.
2.  Traverse the string:
    -   If the character is '1', increment `count`.
    -   If the character is '0', add `(count * (count+1) / 2)` to `ans`
        and reset `count = 0`.
3.  After the loop, add the contribution of the last block of ones.
4.  Print `ans`.

## Complexity Analysis

-   Time complexity: O(\|S\|) since we scan the string once.
-   Memory complexity: O(1) since only a few variables are used.

## Example

Input: `101110`\
Blocks of ones: `"1"` (length 1), `"111"` (length 3).\
Contribution: 1*2/2 = 1 and 3*4/2 = 6.\
Total = 7

Output: `7`

## Implementation (C++)

``` cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    string s;
    cin >> s;

    long long ans = 0, count = 0;
    for (char c : s) {
        if (c == '1') {
            count++;
        } else {
            ans += count * (count + 1) / 2;
            count = 0;
        }
    }
    ans += count * (count + 1) / 2; // last block
    cout << ans << "\n";
}
```
