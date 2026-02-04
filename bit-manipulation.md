# Bit Manipulation for Data Structures & Algorithms

A complete cheat sheet and resource guide for mastering bitwise operations in Competitive Programming and technical interviews.

---

## Table of Contents

- [1. The Core Operators](#1-the-core-operators)
- [2. Basic Bit Tricks (O(1) Shortcuts)](#2-basic-bit-tricks-o1-shortcuts)
- [3. XOR Patterns & Interview Problems](#3-xor-patterns--interview-problems)
- [4. Counting Set Bits](#4-counting-set-bits)
- [5. Gray Code Techniques](#5-gray-code-techniques)
- [6. Subsets and Bitmasking](#6-subsets-and-bitmasking)
- [7. Built-in Functions (GCC)](#7-built-in-functions-gcc)
- [8. Advanced Competitive Programming Tricks](#8-advanced-competitive-programming-tricks)

---

## 1. The Core Operators

| Operation   | Symbol | Logic                | Example            |
| ----------- | ------ | -------------------- | ------------------ |
| AND         | `&`    | 1 if both bits are 1 | `101 & 011 = 001`  |
| OR          | `\|`   | 1 if either bit is 1 | `101 \| 011 = 111` |
| XOR         | `^`    | 1 if bits differ     | `101 ^ 011 = 110`  |
| NOT         | `~`    | Flip all bits        | `~101 = ...111010` |
| Left Shift  | `<<`   | Multiply by $2^k$    | `5 << 1 = 10`      |
| Right Shift | `>>`   | Divide by $2^k$      | `5 >> 1 = 2`       |

---

## 2. Basic Bit Tricks (O(1) Shortcuts)

### Manipulation of k-th Bit

| Goal         | Formula               |
| ------------ | --------------------- |
| Check if set | `(n & (1 << k)) != 0` |
| Set bit      | `n = n \| (1 << k)`   |
| Clear bit    | `n &= ~(1 << k)`      |
| Toggle bit   | `n ^= (1 << k)`       |

### Properties

| Check        | Expression                    |
| ------------ | ----------------------------- |
| Odd          | `(n & 1)`                     |
| Even         | `!(n & 1)`                    |
| Power of 2   | `n > 0 && (n & (n - 1)) == 0` |
| Modulo $2^k$ | `n & ((1 << k) - 1)`          |

### Low-Level Hacks

| Operation                | Trick                       |
| ------------------------ | --------------------------- |
| Clear lowest set bit     | `n & (n - 1)`               |
| Isolate lowest set bit   | `n & (-n)`                  |
| Isolate lowest unset bit | `~n & (n + 1)`              |
| Clear bits LSB → k       | `n & ~((1 << (k + 1)) - 1)` |

---

## 3. XOR Patterns & Interview Problems

### XOR Properties

- `x ^ x = 0`
- `x ^ 0 = x`
- XOR is commutative and associative

---

### Swap Without Temp

```cpp
a ^= b;
b ^= a;
a ^= b;
```

---

### Find Single Unique Number

(All others appear twice)

```cpp
int singleNumber(vector<int>& nums) {
    int ans = 0;
    for(int x : nums)
        ans ^= x;
    return ans;
}
```

---

### Find Missing Number (1 to N)

```cpp
int missingNumber(vector<int>& nums) {
    int n = nums.size();
    int x = 0;

    for(int i = 1; i <= n; i++)
        x ^= i;

    for(int num : nums)
        x ^= num;

    return x;
}
```

---

### Find Two Unique Numbers

(All others appear twice)

Algorithm:

1. XOR all numbers → `x = a ^ b`
2. Find rightmost set bit of `x`
3. Partition numbers using that bit

```cpp
vector<int> twoUnique(vector<int>& nums) {
    int x = 0;
    for(int n : nums) x ^= n;

    int diff = x & (-x);

    int a = 0, b = 0;
    for(int n : nums) {
        if(n & diff) a ^= n;
        else b ^= n;
    }

    return {a, b};
}
```

---

## 4. Counting Set Bits

### Method 1 – Bit Checking

```cpp
int countBits(int n) {
    int c = 0;
    for(int i = 0; i < 32; i++)
        if(n & (1 << i))
            c++;
    return c;
}
```

---

### Method 2 – Brian Kernighan (Best)

```cpp
int countBits(int n) {
    int c = 0;
    while(n) {
        n &= (n - 1);
        c++;
    }
    return c;
}
```

---

## 5. Gray Code Techniques

### Formula Method

```
Gray(i) = i ^ (i >> 1)
```

```cpp
vector<int> grayCode(int n) {
    vector<int> res;
    for(int i = 0; i < (1 << n); i++)
        res.push_back(i ^ (i >> 1));
    return res;
}
```

---

## 6. Subsets and Bitmasking

### Generate All Subsets

```cpp
for(int mask = 0; mask < (1 << n); mask++) {
    for(int i = 0; i < n; i++) {
        if(mask & (1 << i)) {
            // include element i
        }
    }
}
```

---

## 7. Built-in Functions (GCC)

| Function                | Purpose         |
| ----------------------- | --------------- |
| `__builtin_popcount(n)` | Count set bits  |
| `__builtin_ctz(n)`      | Trailing zeros  |
| `__builtin_clz(n)`      | Leading zeros   |
| `__builtin_parity(n)`   | Even/Odd parity |

---

## 8. Advanced Competitive Programming Tricks

### Reverse Bits

```cpp
unsigned int reverseBits(unsigned int n) {
    unsigned int res = 0;
    for(int i = 0; i < 32; i++) {
        res <<= 1;
        res |= (n & 1);
        n >>= 1;
    }
    return res;
}
```

---

### Iterate Submasks (3^N Trick)

```cpp
for (int s = m; s > 0; s = (s - 1) & m) {
    // s is a submask of m
}
```

---
