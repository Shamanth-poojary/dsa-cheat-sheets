# Array Problem-Solving Patterns

This document covers:

- [Two Pointer Technique](#1️⃣-two-pointer-technique)
- [Sliding Window](#2️⃣-sliding-window)
- [Kadane's Algorithm](#3️⃣-kadanes-algorithm)
- [Prefix Sum](#4️⃣-prefix-sum)

For each pattern: - When to use - Requirements - How to apply -
Limitations - Failure cases.

---

# 1️⃣ Two Pointer Technique

## ✅ When to Use

- Array is **sorted** OR can be sorted
- Finding pairs or triplets
- Target sum problems
- Removing duplicates
- Merging sorted arrays
- Comparing elements from both ends
- Partitioning problems

---

## 🔹 Requirements to Apply

✔ Array must be sorted (for sum-based optimization)\
✔ Condition must be monotonic\
✔ Moving pointers must reduce search space logically\
✔ Only local comparison is required

---

## 🔹 Types

### A) Opposite Direction (Left & Right)

Used mainly for sorted arrays.

```cpp
int left = 0;
int right = n - 1;

while (left < right) {
    int sum = arr[left] + arr[right];

    if (sum == target) break;
    else if (sum < target) left++;
    else right--;
}
```

---

### B) Slow & Fast Pointer

Used for: - Remove duplicates - Move zeros - Cycle detection

```cpp
int slow = 0;
for (int fast = 1; fast < n; fast++) {
    if (arr[fast] != arr[slow]) {
        slow++;
        arr[slow] = arr[fast];
    }
}
```

---

## ❌ Limitations

- Fails for unsorted arrays (unless sorting allowed)
- Cannot handle non-monotonic conditions
- Not useful for arbitrary non-contiguous combinations
- Sorting may break index-based problems

---

# 2️⃣ Sliding Window

Used for **contiguous subarray problems**

---

## ✅ When to Use

- Subarray problems
- Maximum/minimum of size k
- Longest substring with condition
- Smallest subarray ≥ target

---

## 🔹 Core Requirement

👉 Elements **MUST** be contiguous.

---

## A) Fixed Size Window

### Requirements

✔ Window size k is given\
✔ Need sum/max/min/count\
✔ Single pass required

```cpp
int windowSum = 0;

for (int i = 0; i < k; i++)
    windowSum += arr[i];

int maxSum = windowSum;

for (int i = k; i < n; i++) {
    windowSum += arr[i];
    windowSum -= arr[i - k];
    maxSum = max(maxSum, windowSum);
}
```

---

## B) Variable Size Window

### Requirements

✔ Condition depends only on window elements\
✔ Window can expand and shrink logically\
✔ Works best when elements are positive

```cpp
int left = 0;

for (int right = 0; right < n; right++) {

    // expand window

    while (condition invalid) {
        left++;  // shrink window
    }

    // update answer
}
```

---

## ❌ Limitations

- Only works for contiguous elements
- Fails when negative numbers break shrinking logic
- Cannot handle non-local constraints
- Cannot select non-adjacent elements

---

# 3️⃣ Kadane's Algorithm

Used for **Maximum Contiguous Subarray Sum**

---

## ✅ When to Use

- Maximum subarray sum
- Minimum subarray sum (modified)
- Circular subarray (modified)

---

## 🔹 Mathematical Requirement

If current prefix sum becomes negative, discard it because it reduces
future sums.

---

## Template

```cpp
int maxSum = arr[0];
int currentSum = 0;

for (int i = 0; i < n; i++) {
    currentSum += arr[i];
    maxSum = max(maxSum, currentSum);

    if (currentSum < 0)
        currentSum = 0;
}
```

---

## ❌ Limitations

- Cannot count number of subarrays
- Cannot enforce fixed-length subarray
- Cannot handle extra constraints (like at least k length)
- Works only for sum/product optimization
- Only for contiguous elements

---

# 4️⃣ Prefix Sum

Used for **Range Queries and Subarray Counting**

---

## ✅ When to Use

- Multiple range sum queries
- Subarray sum = k
- Count subarrays
- Cumulative frequency

---

## 🔹 Requirements

✔ Additive property must exist\
✔ Array should be mostly static (no frequent updates)\
✔ Build prefix array first

---

## 🔹 Build Prefix Array

```cpp
vector<int> prefix(n);
prefix[0] = arr[0];

for (int i = 1; i < n; i++)
    prefix[i] = prefix[i - 1] + arr[i];
```

---

## 🔹 Range Sum Query

```cpp
int rangeSum = prefix[R] - (L > 0 ? prefix[L - 1] : 0);
```

Time: - Build: O(n) - Query: O(1)

---

## 🔹 Subarray Sum = K

```cpp
unordered_map<int,int> mp;
mp[0] = 1;

int sum = 0, count = 0;

for (int i = 0; i < n; i++) {
    sum += arr[i];

    if (mp.find(sum - k) != mp.end())
        count += mp[sum - k];

    mp[sum]++;
}
```

---

## ❌ Limitations

- Not efficient for frequent updates
- Extra O(n) space
- Does not optimize max/min directly
- Requires additive property

---

# 🎯 Master Decision Checklist

Before solving any array problem, ask:

1.  Are elements contiguous?
2.  Is array sorted or sortable?
3.  Is condition monotonic?
4.  Is it maximum/minimum contiguous sum?
5.  Are there multiple range queries?
6.  Are numbers only positive?
7.  Are updates involved?

---

# 🏁 Final Conclusion

These four patterns cover most beginner and intermediate array problems.

But each one has strict requirements.

Apply them only when the required conditions are satisfied. Otherwise,
the approach will fail.
