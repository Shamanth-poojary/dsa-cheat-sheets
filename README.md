# dsa-cheat-sheets
this repo will be used to store some useful dsa concepts and problem solving techniques


# Bit Manipulation for Data Structures & Algorithms

A complete cheat sheet and resource guide for mastering bitwise operations in Competitive Programming and technical interviews.

## Table of Contents
- [1. The Core Operators](#1-the-core-operators)
- [2. Basic Bit Tricks (O(1) Shortcuts)](#2-basic-bit-tricks-o1-shortcuts)
- [3. The "XOR Magic"](#3-the-xor-magic)
- [4. Advanced & Competitive Programming Tricks](#4-advanced--competitive-programming-tricks)

---

## 1. The Core Operators
*The building blocks of all bit manipulation.*

| Operation | Symbol | Logic | Example (Binary) |
| :--- | :---: | :--- | :--- |
| **AND** | `&` | Intersection (1 if both are 1) | `101 & 011 = 001` |
| **OR** | `\|` | Union (1 if either is 1) | `101 \| 011 = 111` |
| **XOR** | `^` | Difference (1 if different) | `101 ^ 011 = 110` |
| **NOT** | `~` | Invert (Flip all bits) | `~101 = ...111010` |
| **Left Shift** | `<<` | Multiply by $2^k$ | `101 << 1 = 1010` (5 -> 10) |
| **Right Shift** | `>>` | Divide by $2^k$ | `101 >> 1 = 10` (5 -> 2) |

---

## 2. Basic Bit Tricks (O(1) Shortcuts)

### Manipulation of k-th Bit
*(0-indexed, from right)*

| Goal | Formula | Explanation |
| :--- | :--- | :--- |
| **Check if set** | `(n & (1 << k)) != 0` | If non-zero, the bit is 1. |
| **Set bit** | `n | (1 << k)` | Forces the $k$-th bit to 1. |
| **Unset (Clear) bit** | `n & ~(1 << k)` | Forces the $k$-th bit to 0. |
| **Toggle bit** | `n ^ (1 << k)` | Flips 0 to 1 and 1 to 0. |

### Properties of Integers
* **Check Odd:** `(n & 1)`
* **Check Even:** `!(n & 1)`
* **Check Power of 2:** `n && !(n & (n - 1))`
* **Modulo $2^k$:** `n & ((1 << k) - 1)` (Equivalent to `n % (2^k)`)

### Low-Level Hacks
* **Clear lowest set bit:** `n & (n - 1)` *(Crucial for counting bits)*
* **Isolate lowest set bit:** `n & (-n)` *(Returns 00...010...00)*
* **Isolate lowest unset bit:** `~n & (n + 1)`
* **Clear all bits from LSB to k-th bit:** `n & ~((1 << (k + 1)) - 1)`

---

## 3. The "XOR Magic"
*XOR is the most common topic for "trick" questions.*

* **Self-Inverse:** `x ^ x = 0`
* **Identity:** `x ^ 0 = x`
* **Swap without temp:**
    ```cpp
    a ^= b;
    b ^= a;
    a ^= b;
    ```
* **Find Unique Number:** (All numbers appear twice except one)
    ```cpp
    int unique = 0;
    for(int x : nums) unique ^= x;
    return unique;
    ```
* **Find Missing Number (1 to N):** `XOR(1...N) ^ XOR(Array)`

---

## 4. Advanced & Competitive Programming Tricks

### Built-in Functions (GCC/C++)
*Hardware-optimized functions. For `long long`, append `ll` (e.g., `__builtin_popcountll`).*

* `__builtin_popcount(n)`: Count number of 1s.
* `__builtin_clz(n)`: Count Leading Zeros.
* `__builtin_ctz(n)`: Count Trailing Zeros.

### Iterating Submasks (The $3^N$ Trick)
*Crucial for "Sum over Subsets" (SOS) DP. Iterates only bits set in `m`.*
```cpp
for (int s = m; s > 0; s = (s - 1) & m) {
    // 's' is a submask of 'm'
}
