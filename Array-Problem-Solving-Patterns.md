# 🧠 Array Problem-Solving Patterns (Interview Ready)

This document covers the most important array patterns used in:

- Striver A–Z Sheet
- Coding Interviews
- Competitive Programming
- Placement Rounds

---

## 📌 Patterns Covered

1. Two Pointer Technique  
2. Sliding Window  
3. Kadane’s Algorithm  
4. Prefix Sum  
5. Binary Search  
6. Dutch National Flag  
7. Moore’s Voting Algorithm  
8. Difference Array  
9. Rotate Array  
10. Merge Intervals  
11. Trapping Rainwater  

---

# 1️⃣ Two Pointer Technique

## ✅ When to Use

- Sorted array
- Pair/Triplet problems
- Target sum
- Removing duplicates
- Partitioning problems

---

## 🧠 Idea

Move two pointers to reduce search space logically.

---

## 🔹 C++ Template

```cpp
int left = 0, right = n - 1;

while (left < right) {
    int sum = arr[left] + arr[right];

    if (sum == target) break;
    else if (sum < target) left++;
    else right--;
}
```

---

## 🔹 Java Template

```java
int left = 0, right = arr.length - 1;

while (left < right) {
    int sum = arr[left] + arr[right];

    if (sum == target) break;
    else if (sum < target) left++;
    else right--;
}
```

---

## 📚 Striver Problems

- Two Sum (Sorted)
- 3Sum
- Container With Most Water
- Remove Duplicates
- Trapping Rainwater

---

## ❌ Limitations

- Requires sorting
- Fails for non-monotonic conditions

---

# 2️⃣ Sliding Window

Used for **contiguous subarray problems**

---

## A) Fixed Size Window

### C++

```cpp
int windowSum = 0;
for(int i=0;i<k;i++)
    windowSum += arr[i];

int maxSum = windowSum;

for(int i=k;i<n;i++){
    windowSum += arr[i] - arr[i-k];
    maxSum = max(maxSum, windowSum);
}
```

---

### Java

```java
int windowSum = 0;

for(int i=0;i<k;i++)
    windowSum += arr[i];

int maxSum = windowSum;

for(int i=k;i<arr.length;i++){
    windowSum += arr[i] - arr[i-k];
    maxSum = Math.max(maxSum, windowSum);
}
```

---

## 📚 Striver Problems

- Maximum Subarray of Size K
- Longest Substring Without Repeating Characters
- Smallest Subarray ≥ K

---

## ❌ Limitations

- Works only for contiguous elements
- Negative numbers may break shrinking logic

---

# 3️⃣ Kadane’s Algorithm

## 🧠 Maximum Subarray Sum

---

### C++

```cpp
int maxSum = arr[0];
int currentSum = 0;

for(int i=0;i<n;i++){
    currentSum += arr[i];
    maxSum = max(maxSum, currentSum);

    if(currentSum < 0)
        currentSum = 0;
}
```

---

### Java

```java
int maxSum = arr[0];
int currentSum = 0;

for(int i=0;i<arr.length;i++){
    currentSum += arr[i];
    maxSum = Math.max(maxSum, currentSum);

    if(currentSum < 0)
        currentSum = 0;
}
```

---

## 📚 Striver Problems

- Maximum Subarray
- Maximum Circular Subarray
- Largest Sum Contiguous Subarray

---

## ❌ Limitations

- Only for contiguous
- Cannot enforce fixed length

---

# 4️⃣ Prefix Sum

Used for **range queries and subarray count**

---

### C++

```cpp
vector<int> prefix(n);
prefix[0] = arr[0];

for(int i=1;i<n;i++)
    prefix[i] = prefix[i-1] + arr[i];
```

---

### Java

```java
int[] prefix = new int[n];
prefix[0] = arr[0];

for(int i=1;i<n;i++)
    prefix[i] = prefix[i-1] + arr[i];
```

---

## Subarray Sum = K (C++)

```cpp
unordered_map<int,int> mp;
mp[0] = 1;

int sum = 0, count = 0;

for(int i=0;i<n;i++){
    sum += arr[i];
    if(mp.count(sum-k))
        count += mp[sum-k];
    mp[sum]++;
}
```

---

## 📚 Striver Problems

- Subarray Sum Equals K
- Count Subarrays With Given XOR
- Range Sum Query

---

# 5️⃣ Binary Search

### C++

```cpp
int low=0, high=n-1;

while(low<=high){
    int mid = low + (high-low)/2;

    if(arr[mid]==target)
        return mid;
    else if(arr[mid]<target)
        low=mid+1;
    else
        high=mid-1;
}
```

---

### 📚 Striver Problems

- First and Last Occurrence
- Search in Rotated Sorted Array
- Peak Element

---

# 6️⃣ Dutch National Flag

Used for sorting 0,1,2

```cpp
int low=0, mid=0, high=n-1;

while(mid<=high){
    if(arr[mid]==0){
        swap(arr[low],arr[mid]);
        low++; mid++;
    }
    else if(arr[mid]==1)
        mid++;
    else{
        swap(arr[mid],arr[high]);
        high--;
    }
}
```

---

# 7️⃣ Moore’s Voting Algorithm

```cpp
int count=0, candidate;

for(int num:arr){
    if(count==0)
        candidate=num;

    if(num==candidate)
        count++;
    else
        count--;
}
```

---

# 8️⃣ Difference Array

```cpp
vector<int> diff(n+1,0);

diff[l]+=val;
diff[r+1]-=val;

for(int i=1;i<n;i++)
    diff[i]+=diff[i-1];
```

---

# 9️⃣ Rotate Array (Reversal)

```cpp
reverse(arr.begin(), arr.end());
reverse(arr.begin(), arr.begin()+k);
reverse(arr.begin()+k, arr.end());
```

---

# 🔟 Merge Intervals

```cpp
sort(intervals.begin(), intervals.end());

for(auto interval:intervals){
    if(result.empty() || result.back()[1]<interval[0])
        result.push_back(interval);
    else
        result.back()[1]=max(result.back()[1],interval[1]);
}
```

---

# 1️⃣1️⃣ Trapping Rainwater

```cpp
int left=0,right=n-1;
int leftMax=0,rightMax=0,water=0;

while(left<right){
    if(arr[left]<arr[right]){
        if(arr[left]>=leftMax)
            leftMax=arr[left];
        else
            water+=leftMax-arr[left];
        left++;
    }
    else{
        if(arr[right]>=rightMax)
            rightMax=arr[right];
        else
            water+=rightMax-arr[right];
        right--;
    }
}
```

---

# 📊 Complexity Summary

| Pattern | Time | Space |
|----------|------|--------|
| Two Pointer | O(n) | O(1) |
| Sliding Window | O(n) | O(1) |
| Kadane | O(n) | O(1) |
| Prefix Sum | O(n) | O(n) |
| Binary Search | O(log n) | O(1) |
| Dutch Flag | O(n) | O(1) |
| Moore Voting | O(n) | O(1) |

---

# 🧠 Master Decision Checklist

Before solving:

1. Is array sorted?
2. Are elements contiguous?
3. Is it a target sum problem?
4. Is it maximum/minimum contiguous sum?
5. Are there multiple range queries?
6. Are updates involved?
7. Is majority guaranteed?

---

# 🏁 Final Note

Choosing the correct pattern is more important than writing code.

Most Striver A–Z array problems are combinations of these techniques.

Master the pattern → Coding becomes mechanical.