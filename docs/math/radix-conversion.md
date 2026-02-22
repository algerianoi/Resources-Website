# Radix Conversion (Number Bases)

## Introduction
Radix conversion, also known as base conversion, is the process of converting a number from one base (radix) to another. Common bases include binary (base 2), octal (base 8), decimal (base 10), and hexadecimal (base 16).

## What is a number base?
A number base is a way of representing numbers using a specific set of digits. The base determines how many unique digits are used and the value of each position in a number. For example:

### Base-10 (Decimal)
In base 10 (decimal), the digits are 0-9, and each position represents a power of 10.
If a number is written as $d_n d_{n-1} ... d_1 d_0$ in base $10$, its value can be calculated as:
$$\text{Value} = d_n \cdot 10^n + d_{n-1} \cdot 10^{n-1} + ... + d_1 \cdot 10^1 + d_0 \cdot 10^0$$

Example: The number 345 in base 10 can be calculated as:
$$3 \cdot 10^2 + 4 \cdot 10^1 + 5 \cdot 10^0 = 300 + 40 + 5 = 345$$

### Base-2 (Binary)
In binary (base 2), the digits are 0 and 1, and each position represents a power of 2. If a number is written as $d_n d_{n-1} ... d_1 d_0$ in base $2$, its value can be calculated as:
$$\text{Value} = d_n \cdot 2^n + d_{n-1} \cdot 2^{n-1} + ... + d_1 \cdot 2^1 + d_0 \cdot 2^0$$

### Base-8 (Octal)
Now if we are working in base 8 (octal), the digits are 0-7, and each position represents a power of 8. If a number is written as $d_n d_{n-1} ... d_1 d_0$ in base $8$, its value can be calculated as:
$$\text{Value} = d_n \cdot 8^n + d_{n-1} \cdot 8^{n-1} + ... + d_1 \cdot 8^1 + d_0 \cdot 8^0$$

### Base-16 (Hexadecimal)
Last but not least, in base 16 (hexadecimal), the digits are 0-9 and A-F (where A=10, B=11, ..., F=15), and each position represents a power of 16. If a number is written as $d_n d_{n-1} ... d_1 d_0$ in base $16$, its value can be calculated as:
$$\text{Value} = d_n \cdot 16^n + d_{n-1} \cdot 16^{n-1} + ... + d_1 \cdot 16^1 + d_0 \cdot 16^0$$

### General Case 
In base $b$, a number written as $d_n d_{n-1} ... d_1 d_0$ has value:
$$\text{Value} = d_n \cdot b^n + d_{n-1} \cdot b^{n-1} + ... + d_1 \cdot b^1 + d_0 \cdot b^0$$


So if you are given a number in base $b$ and want to convert it to decimal, you can use the above formula to calculate its value. The algoithm for converting a number from base $b$ to decimal is as follows:
1. Initialize a variable `result` to 0.
2. For each digit `d` in the number (starting from the leftmost digit):
    - Update `result` as `result = result * b + d`.
3. Return `result` as the decimal representation of the number.

## Converting from Decimal to Another Base
This part is the trickier one. Let's do it step by step.
First, let's figure out what is the first digit of the number in the new base, i.e the least significant digit or rightmost one. We can find it by taking the remainder of the number when divided by the new base. For example, if we want to convert the decimal number 45 to binary (base 2), we can find the least significant digit as follows:
$$45 \mod 2 = 1$$
So the least significant digit in binary is 1. Next, we can update the number by dividing it by the new base and taking the integer part. This will give us the remaining part of the number that we need to convert. For our example:
$$45 \div 2 = 22$$
Now we can repeat the process with the updated number. We take the remainder again:
$$22 \mod 2 = 0$$
So the next digit in binary is 0. We update the number again:
$$22 \div 2 = 11$$
We repeat the process:
$$11 \mod 2 = 1$$
$$11 \div 2 = 5$$
$$5 \mod 2 = 1$$
$$5 \div 2 = 2$$
$$2 \mod 2 = 0$$
$$2 \div 2 = 1$$
$$1 \mod 2 = 1$$
$$1 \div 2 = 0$$
Now we have all the digits in binary: 101101. We can reverse the order of the digits to get the final result: 110101. So the decimal number 45 is equal to 110101 in binary.

The algorithm for converting a decimal number to another base is as follows:
1. Initialize an empty string `result` to store the digits of the new base.
2. While the number is greater than 0:
    - Find the least significant digit by taking the remainder of the number when divided by the new base: `digit = number % base`.
    - Append the digit to the `result` string (you may need to convert it to the appropriate character if the digit is greater than 9).
    - Update the number by dividing it by the new base and taking the integer part: `number = number // base`.
3. Reverse the `result` string to get the final representation in the new base.

## C++ Implementation
Here is a C++ implementation of the above algorithms for converting between decimal and another base:
```cpp
#include <iostream>
#include <string>
#include <algorithm>
using namespace std;
// Function to convert a number from base b to decimal
int toDecimal(const string& number, int base) {
    int result = 0;
    for (char digit : number) {
        result = result * base + (isdigit(digit) ? digit - '0' : digit - 'A' + 10);
    }
    return result;
}
// Function to convert a decimal number to base b
string fromDecimal(int number, int base) {
    string result;
    while (number > 0) {
        int digit = number % base;
        result += (digit < 10) ? (digit + '0') : (digit - 10 + 'A');
        number /= base;
    }
    reverse(result.begin(), result.end());
    return result;
}
int main() {
    string number;
    int base;
    cout << "Enter a number and its base: ";
    cin >> number >> base;
    int decimalValue = toDecimal(number, base);
    cout << "Decimal value: " << decimalValue << endl;
    cout << "Enter a decimal number and the base to convert to: ";
    cin >> decimalValue >> base;
    string convertedValue = fromDecimal(decimalValue, base);
    cout << "Converted value: " << convertedValue << endl;
    return 0;
}
```
This code defines two functions: `toDecimal` for converting a number from a given base to decimal, and `fromDecimal` for converting a decimal number to a specified base. The `main` function demonstrates how to use these functions by taking user input for both conversions.

## External Resources
- Pages 95 and 96 of Competitive Programming Handbook by Antti Laaksonen.
- [Number base concepts](https://adacomputerscience.org/concepts/number_intro_concepts)
- [Number Systems Introduction - Decimal, Binary, Octal & Hexadecimal](https://www.youtube.com/watch?v=FFDMzbrEXaE)
- [France-IOI - Bases](https://www.france-ioi.org/algo/chapter.php?idChapter=565)

## Problems
- [LeetCode - Count the Digits That Divide a Number](https://leetcode.com/problems/count-the-digits-that-divide-a-number/description/?envType=problem-list-v2&envId=math)
- [LeetCode - Hexadecimal and Hexatrigesimal Conversion](https://leetcode.com/problems/hexadecimal-and-hexatrigesimal-conversion/description/)
- [LeetCode - Convert to Base-2](https://leetcode.com/problems/convert-to-base-2/description/)
- [LeetCode - Sum of Digits in Base K](https://leetcode.com/problems/sum-of-digits-in-base-k/description/)
- [LeetCode - Base 7](https://leetcode.com/problems/base-7/description/)
- [LeetCode - Smallest Good Base](https://leetcode.com/problems/smallest-good-base/description/)
- [LeetCode - Find Nth Smallest Integer With K One Bits](https://leetcode.com/problems/find-nth-smallest-integer-with-k-one-bits/description/)
