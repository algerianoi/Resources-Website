# The Dripping Tap
*Written by Mina Ragy*

## Problem Statement:
- [English](https://naoi2025.algerianoi.com/statements/dripping%20(en).pdf)
- [Arabic](https://naoi2025.algerianoi.com/statements/dripping%20(ar_DZ).pdf)
- [French](https://naoi2025.algerianoi.com/statements/dripping%20(fr).pdf)


## Insights
In problems with such setups (scoring formula suggesting somewhat logarithmic solutions, unknown value of some integer $n$, etc), it might be useful to try to work with binary representations.
    
A simple algorithm to find the binary representation of some integer $x$ would be
```cpp
for (int i = 0; i < 10; i++):
    binary_representation.append(x % 2); //identifies the i-th bit of x
    x /= 2; //floor division
```

This is the algorithm we will be using to determine the binary representation of the number of prisoners (refered to as $n$ in the editorial). In other words, we need to find a way to perform lines $2$ and $3$ from the algorithm using the problem's setup.


We will use the whiteboard to identify each bit of $n$, and it will only have bits written on it throughout the whole interaction (i.e. $M = 1$).

It can be noticed that the only new piece of information each prisoner has whenever they enter the room is the value written on the whiteboard, so it makes sense that each prisoner will always write the value they saw on the board on their clothes.

## 1. Determining the parity of $n$ (value of $n \bmod 2$)
To determine the parity of $n$ (line $2$ of the algorithm), in the first cycle, we can make each prisoner flip the bit on the whiteboard. This way, after the first cycle, the number on the whiteboard will be $n \bmod 2$.

## 2. Division by $2$
To perform $n \gets \displaystyle \left \lfloor \frac{n}{2} \right \rfloor$ (line $3$ of the algorithm), we need to notice that if we number the prisoners $0, 1, \dots, n - 1$, the first number written in prisoner $i$'s notebook will be $i \bmod 2$.

To divide $n$ by $2$, we need to somehow ignore half of the prisoners in future cycles. We should notice now that we have the prisoners divided into two sets:
- Set $A$ consisting of prisoners $i$ such that $i \equiv 0 \pmod 2$
- Set $B$ consisting of prisoners $i$ such that $i \equiv 1 \pmod 2$

Set $A$ would be of size $\displaystyle \left \lceil \frac{n}{2} \right \rceil$ and set $B$ would be of size $\displaystyle \left \lfloor \frac{n}{2} \right \rfloor$.

Since, in line $2$, we want $n \gets \displaystyle \left \lfloor \frac{n}{2} \right \rfloor$, therefore, if we ignore prisoners from set $A$ in future cycles (i.e. treat them as if they don't exist and leave everything on the whiteboard as it is) and only consider those from set $B$ in future cycles that determine the parity after the division, this would have the same effect as $n \gets \displaystyle \left \lfloor \frac{n}{2} \right \rfloor$. This can be done by looking through each prisoner's notebook.

## 3. Getting the binary representation of $n$
Notice that after the first cycle, the whiteboard would have the value of $n \bmod 2$. So if you repeat the same described algorithm again that determines the values of $n \bmod 2$ and $i \bmod 2$ immediately after the first cycle, you won't know the actual value of $i \bmod 2$ for each prisoner $i$, you will know $(i \bmod 2)\ \text{xor}\ (n \bmod 2)$. So we need to consider this.

We will be discussing two solutions.

### $C = 19.001$:
By announcing the value of $n \bmod 2$ to all prisoners in an additional cycle every time, each prisoner $i$ will know the value of $(i \bmod 2)\ \text{xor}\ (n \bmod 2)$ and $n \bmod 2$, which would allow them to determine the value of $i \bmod 2$.

So the solution would be:
1. Make a cycle to determine the parity of $n$ as explained above.
2. Make a cycle to announce the parity of $n$ to all prisoners.
3. If you still don't know all the bits repeat step $1$ while only considering prisoners from set $B$.

This takes $2 \cdot \log_2 n - 1 + \frac{1}{n}$ cycles.

**Implementation**
```cpp
tuple<char, ll, ll> prisonier(ll w, vector<ll> a) {
	int x, y;
	if (SZ(a) & 1) { //The value of n mod 2 is being announced
		x = w;
		y = (a.back() ^ w);
	} else { //flip the parity
		bool ok = 1;
		for (int k = 1; k < SZ(a); k += 2) {
			ok &= a[k] == 0; //check that the prisoner belongs to set B
		}
		x = (w ^ ok); //flip w iff the prisoner belongs to set B
		y = w;
	}
	if (SZ(a) == 19) { //the prisoner knows all bits
		a.push_back(y);
		int ans = 0;
		for (int i = SZ(a) - 1; i >= 0; i -= 2) {
			if (i >= 3) {
				a[i - 1] ^= a[i - 2] ^ a[i - 3];
			}
			ans = (ans << 1) | (a[i - 1] ^ a[i]);
		}
		return {'a', ans, 0};
	}
	return {'w', x, y};
}
```

### $C = 11.001$:
We need to notice that there is some sort of redundancy in the previous algorithm. We don't actually need all prisoners to know the binary representation of $n$, one is enough. So we can pick a leader who would have the necessary information to know the binary representation of $n$.

Prisoner $0$ is the first prisoner to see the parity of $n$ after each cycle, so it makes sense to choose him as the leader. This way, instead of announcing the value of $n \bmod 2$ to everyone, it will be sufficient for the leader only to know it. In addition, we will make the leader reset the value on the whiteboard to $0$ (and possibly flip it afterwards) after knowing the value of $n \bmod 2$, which would allow other prisoners to correctly determine the value of $i \bmod 2$.

So the solution would be:

1. Make a cycle to tell prisoner $0$ that he is the leader and other prisoners that they are not.
2. Make a cycle to determine the parity of $n$.
3. If you still don't know all the bits repeat step $2$ while only considering prisoners from set $B$.

This takes $\log_2 n + 1 + \frac{1}{n}$ cycles.

```cpp
tuple<char, ll, ll> prisonier(ll w, vector<ll> a) {
	int x, y = w;
	if (SZ(a) == 0) { //the cycle that tells prisonners who is the leader
		x = 1;
	} else {
		if (a[0] == 0) {
			x = 0; //the leader resets the whiteboard
            if (SZ(a) == 1) x = 1;
		} else {
			bool ok = 1;
			for (int i = 1; i < SZ(a); i++) {
				ok &= (a[i] == 1); //check that the prisoner belongs to set B
			}
			x = (w ^ ok); //flip w iff the prisoner belongs to set B
		}
	}
	if (SZ(a) == 11) { //the prisoner knows all bits
		a.push_back(y);
		int ans = 0;
		for (int i = 2; i < SZ(a); i++) {
			ans |= (a[i] << (i - 2));
		}
		return {'a', ans, 0};
	}
	return {'w', x, y};
}
```